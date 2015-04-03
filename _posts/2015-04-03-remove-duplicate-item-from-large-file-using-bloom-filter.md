---
layout: post
title: 用布隆过滤器给大文件去重
category: algorithm
description: "大文件去重，这种事情往往会让人烦恼，想在合理的内存空间内，合理的利用CPU，那么布隆过滤器不失为一种不错的选择..."
modified: 2015-04-03
tags: [algorithm]
image:
    background: symphony.png
---

大文件去重这种问题，记得以前校招面试的时候曾经遇到过，基本思路无外乎分而治之，hash一下等等，布隆过滤器了解的也不多，只能说是听说过这等神奇。今天来看看给大文件去重上，他能发挥多大力量吧。


  分而治之的方案大概来说就是把文件先分割为多个小文件，然后小文件之间进行merge，相同的跳过。然后依次递归进行。如果合并后的文件大小超过合并文件的大小限制，就再分割。Hash的做法是如果每个item比较大，那么就hash下，对hash值进行去重操作。

  Hash还有另外一种比较高效的做法，就是对每一个要去重的item计算一个md5值出来，再构建一个Trie树，再插入进来的md5如果存在于这个树里就抛弃这个item，继续下一个。这个效率也是比较高的，但是可能做法就比较复杂，耗时也会相对较长，对于item重复度比较高的情况，有不错的表现。但是如果重复度比较低，则耗时耗内存。

  对于重复度不高，要求速度快，空间省，可以容忍一些小差误的情况下，布隆过滤器是一个很好的选择。最简单的例子：在制作一个网络爬虫程序时，URL错综复杂，可能会构成环。为了避免形成环，我们需要知道是不是访问过某个URL，如果用hash表来存储的话，巨大的hash表内存很有可能存不下，使用布隆过滤器就能在保证空间和时间性能的情况下，完成这一任务。

###原理

>创建一个m位BitSet，先将所有位初始化为0，然后选择k个不同的哈希函数。第i个哈希函数对字符串str哈希的结果记为h（i，item），且h（i，item）的范围是0到m-1，然后将对应位的BitSet置为1。

<figure>
	<a href="https://raw.githubusercontent.com/lonelyswan/lonelyswan.github.io/master/images/649px-Bloom_filter.svg.png"><img src="https://raw.githubusercontent.com/lonelyswan/lonelyswan.github.io/master/images/649px-Bloom_filter.svg.png" alt=""></a>
	<figcaption>The false positive probability p as a function of number of elements n in the filter and the filter size m. An optimal number of hash functions k= (m/n) \ln 2 has been assumed.</figcaption>
</figure>

* Add：要添加一个元素，使用K个hash函数将item Hash后Bloom Filter中的K个bit位置为1
* Query：要查询一个元素，使用K个hash函数将item Hash后的K个bit位与Bloom Filter中的相应位置比较，如果全部为1，则该item出现过，如果有任何一位不为1则该item没有出现过。
* Remove：不允许删除元素。因为要remove的item的K个hash位可能也被其他item的位所共用，所以不允许删除元素。

当然如果Bloom Filter中的元素n过多，导致n/m过大时错误率就会相应提高。

###误判

误判可能是我们使用布隆过滤器时最担心的问题，那我们就来算算误判概率大概是多少吧。枯燥的数学计算，不喜欢就跳过吧，没啥意义。

假设我们使用的hash函数能够等概率的把item hash到m个bit位里面去。

那么某一位没有被置为1的概率是$$ 1- \frac{1}{m} $$。

k个hash函数都没有给某一位置为1的概率是$$ \left(1- \frac{1}{m}\right)^k $$

插入了n个元素以后这个位依然没有被置为1的概率是$$ \left(1- \frac{1}{m}\right)^{nk} $$

那么插入了n个元素以后这个位被置为1的概率就是$$ 1- \left(1- \frac{1}{m}\right)^{nk} $$

在查询一个元素是否存在时，假设这个元素从没有出现过，但是hash得到的k个位全部为1的概率就是$$ p(error) = \left(1- \left(1- \frac{1}{m}\right)^{nk}\right)^k $$

下面高数终于能派上点用场了...化简这个式子

$$ \left(1- \left(1- \frac{1}{m}\right)^{nk}\right)^k = \left(1- \left(1+\frac{1}{-m}\right)^{-m\frac{-kn}{m}}\right)^{k}$$

在$$ x \rightarrow 0 $$时，有 $$ (1+x)^{\frac{1}{x}} = e $$则：

$$ \left(1- \left(1- \frac{1}{m}\right)^{nk}\right)^k = \left(1- e^{\frac{nk}{-m}}\right)^{k}$$

要算在k为多少的情况下误判率最少，还要求导，还要求最值，估计我写了也没人爱看，直接从书上copy答案了。

在$$ k = 2^{- \ln 2\frac{m}{n}} \approx 0.618^{\frac{m}{n}}$$

有了这个式子就可以根据自己期望的误判率和预估的需要判重的item的量来确定给定多少位bit分配多大的Bloom Filter。

假设我们有***一百万***条数据需要判重，如果需要错误率低于1%，那么我们可以算出来。需要不到一千万bit和7个hash函数。也就是不到***10MB***的空间就可以完成。如果要求0.1%的错误率也只需要***14M***空间和10个hash函数。这些hash函数是互相不相关的可以并行的运算，更是提高了速度。

###实现

粘一段很简单的实现：参考<a href="http://www.cnblogs.com/hitwtx/archive/2011/08/24/2152180.html">这里</a>

{% highlight java %}

import java.util.BitSet;
public class  SimpleBloomFilter {
     private static final  int  DEFAULT_SIZE  =2 << 24 ;
     private static final  int [] seeds =new  int []{5,7, 11 , 13 , 31 , 37 , 61};
     private  BitSet bits= new  BitSet(DEFAULT_SIZE);
     private  SimpleHash[]  func=new  SimpleHash[seeds.length];   
     public  SimpleBloomFilter() {
         for( int  i= 0 ; i< seeds.length; i ++ ) {
            func[i]=new  SimpleHash(DEFAULT_SIZE, seeds[i]);
        }
    }
     public void  add(String value) {
         for(SimpleHash f : func) {
            bits.set(f.hash(value),  true );
        }
    }
     public boolean  contains(String value) {
         if(value ==null ) {
             return false ;
        }
         boolean  ret  = true ;
         for(SimpleHash f : func) {
            ret=ret&& bits.get(f.hash(value));
        }
         return  ret;
    }
     
     //内部类，simpleHash
     public static class SimpleHash {
         private int  cap;
         private int  seed;
         public  SimpleHash( int cap, int seed) {
             this.cap= cap;
             this.seed =seed;
        }
         public int hash(String value) {
             int  result=0 ;
             int  len= value.length();
             for  (int i= 0 ; i< len; i ++ ) {
                result =seed* result + value.charAt(i);
            }
             return (cap - 1 ) & result;
        }
    }
     
     public static void  main(String[] args) {
         String value  = "stone2083@yahoo.cn" ;
         SimpleBloomFilter filter=new  SimpleBloomFilter();
         System.out.println(filter.contains(value));
         filter.add(value);
         System.out.println(filter.contains(value));
     }
}

{% endhighlight %}

<div markdown="0"><a href="http://en.wikipedia.org/wiki/Bloom_filter" class="btn btn-info">Wiki --Bloom Filter</a></div>

<div markdown="0"><a href="https://github.com/MagnusS/Java-BloomFilter/blob/master/src/com/skjegstad/utils/BloomFilter.java" class="btn btn-info">Github --Bloom Filter</a></div>

