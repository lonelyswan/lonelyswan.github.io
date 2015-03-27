---
layout: post
title: 内存不够？SpringMVC流式上传文件
category: Spring
description: "Spring流式上传文件"
modified: 2015-03-20
tags: [Spring,file,upload]
image:
    background: witewall_3.png
---


上周工作中遇到一个问题，需要上传大文件来满足一个需求。大文件占内存啊，怎么搞呢？流式上传是一个颇为不错的解决方案。

先说说常用的两种文件上传方式：

1. 使用CommonsMultipartFile中的transferTo方法来保存文件
2. 使用CommonsMultipartResolver然后使用MultipartFile来接这个文件

先上个代码

{% highlight java %}
@RequestMapping(value = "fileUpload", method = RequestMethod.POST) 
@ResponseBody 
public Object fileUpload(@RequestParam("fileUpload") CommonsMultipartFile file) {  
        if (!file.isEmpty()) {  
        String path = "root" + file.getOriginalFilename();  
        File localFile = new File(path);  
        try {  
            file.transferTo(localFile);  
        } catch (Exception e) {  
            e.printStackTrace();  
        } 
    }  
    return new sRespones("success");  
} 

@RequestMapping(value = "fileup", method = RequestMethod.POST)
@ResponseBody   
public String fileUp(HttpServletRequest request)  throws Exception{  
        CommonsMultipartResolver multipartResolver = new CommonsMultipartResolver(request.getSession().getServletContext());
        if (multipartResolver.isMultipart(request)) {  
            MultipartHttpServletRequest multiRequest = (MultipartHttpServletRequest) request;  
            Iterator<String> iter = multiRequest.getFileNames();  
            while (iter.hasNext()) {  
            MultipartFile file = multiRequest.getFile(iter.next());  
            if (file != null) {  
                String fileName = "up" + file.getOriginalFilename();  
                String path = root + fileName;  
                File localFile = new File(path);  
                file.transferTo(localFile);  
            }  
            }  
       }  
    return new sRespones("success");
}  
{% endhighlight %}

上面两段代码就是常用的两种方式。
但是仔细分析一下*transferTo()*这个函数就会发现问题，*transferTo()*函数调用*write()*函数而这个函数要求上传文件已经要在内存中，或者是在磁盘里，才能成功执行。

上面第二段代码里直接用了*MultipartFile*，从这个接口的说明里面直接copy出来一段说明

> The file contents are either stored in memory or temporarily on disk.
> In either case, the user is responsible for copying file contents to a
> session-level or persistent store as and if desired. The temporary storages
> will be cleared at the end of request processing.
  
这段话直接说明了，这是需要存储在内存或者磁盘里面的。所以这两种上传方式不适合上传大文件。如果文件过大则容易损耗过多的内存，非常不划算。

流式上传的优势就体现出来了，省内存，但是速度会比较慢。还是上一段代码
{% highlight java %}
@RequestMapping(value="/upload",  method= RequestMethod.POST)
@ResponseBody
public Object fileUpload(@RequestParam("file") CommonsMultipartFile file)
{
try {
     InputStream is = file.getInputStream();
     InputStreamReader ris = new InputStreamReader(is);
     BufferedReader br = new BufferedReader(ris);
     ///**一行行写文件的操作***///
     ///***写完了把流关上***///
     }catch(Exception e){
     e.printStackTrace();
     }
}
{% endhighlight %}

与之前第一种方法写的非常相似，唯一的区别不过在于不是使用*transferTo()*来接受文件而是使用流来接受文件，这样就可以不断的清空流的内容，保证内存的占用不会提高。




