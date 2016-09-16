#OC中去掉网页中指定元素
1.先看需求(图片1)，去掉底部秀米技术提供一栏
![1.png](/Users/warron/Desktop/OC与JS 去除网页中特定的div元素/1.png)

2.通过花瓶抓包，得到返回的html代码

![2.png](/Users/warron/Desktop/OC与JS 去除网页中特定的div元素/2.png)

- 图中红框内代码就是待去除的代码，注意划横线是这个session的的唯一编码标示

3.新建一个文件，编写js代码，文件名后缀是js

![3.png](/Users/warron/Desktop/OC与JS 去除网页中特定的div元素/3.png)
- 相关代码
```
 function deleteBottomElements() {
    var allElements = document.getElementsByTagName('section');
    for (var i=0; i< allElements.length; i++ )
    {
        if (allElements[i].className == 'row tn-board-foot' ) {
            var p = allElements[i].parentNode;
            p.removeChild(allElements[i]);
        } 
    }
}
```


- document.getElementsByTagName('section')，这个session是待去除部分的标签名
- allElements[i].className == 'row tn-board-foot'  是待去除部分的唯一标示

4. 在webViewDidFinishLoad代理方法中完成注入js代码

![4.png](/Users/warron/Desktop/OC与JS 去除网页中特定的div元素/4.png)
- 相关代码（记得加上代理，如_webView.delegate = self）
```
-(void)webViewDidFinishLoad:(UIWebView *)web{
    
    [_webview stringByEvaluatingJavaScriptFromString:[NSString stringWithContentsOfURL:[[NSBundle mainBundle] URLForResource:@"removeBottom" withExtension:@"js"] encoding:NSUTF8StringEncoding error:nil]];
    
    [_webview  stringByEvaluatingJavaScriptFromString:@"deleteBottomElements()"];
    
    [[QJ lb] hideLoading];
}
```
5.最终效果图

![5.png](/Users/warron/Desktop/OC与JS 去除网页中特定的div元素/5.png)

6.参考链接
http://www.tuicool.com/articles/6nM3ym
http://blog.sina.com.cn/s/blog_693de6100102vi3w.html