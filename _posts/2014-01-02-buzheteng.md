---
layout: post
title: "不折腾了,只更新文章"
categories:
- 扯淡
tags:
- 扯淡

---

这几天一直在折腾博客，注册了免费的tk顶级域名，搭建这样的小窝完全可以不花钱，但是要承认花少许的钱就可以做的比这好得多。当然大部分都是在改界面，各种暴力修改css，html，改到我比较满意的样子，至少仍然简洁，暂时就这样了，再改下去估计就要重构代码了，对比一下：  
![](/media/png/myblog.png) 

![](/media/png/webfblog.png) 

自我感觉比模板的好看，比如测试一下代码区块`hello,world`
{% highlight c %}
#include <stdio.h>
#include <stdlib.h>
int main(){
	int jia(int x,int y);
	int a,b,i;
	printf("hello,world !\n");
	a=34;b=38;
	printf("the %d + %d 's return is %d\n",a,b,jia(a,b));
	return 0;			
	  }
int jia(int x,int y)
       {	
	return (x+y); 
       }
{% endhighlight %}
不过这些有什么实际意义吗，如果根本就没有文章。。。  
之前没有css或html基础，通过找资料，暴力修改也能学到不少东西→ _→  
最后决定把博客托管gitcafe上了，因为github在国内的访问速度有些慢。
