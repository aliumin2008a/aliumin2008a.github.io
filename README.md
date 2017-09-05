

## 在进行UI自动化调试时候遇到的问题 结合自身封装的方法
# 获取WebElement对应的文本
>例如下面的html片段 要获取input中的value
````Html
<div class="am-input-control">
    <input type="text" value="你好">
</div>
````
>如果得到的文本只为空,而非我们期望的"你好".那么尝试使用WebElement.isDisplayed()时候,将会得到false的结果.再尝试使用getAttribute("value"),发现能够争取获取value的值.由此可以说明:
1、WebDriver判定isDisplayed为false的元素,那么getText()将为空
2、isDisplayed为false的元素,依然可以通过getAttribute()方法获取元素的属性.
````Java
String text = WebElement.getAttribute("value");
````
上述Text的值就是“你好”
所以,当getText()为空的时候,可以通过两种方法获取链接的文本
1、修改页面当前元素,或者当前元素父元素的CSS,使元素的isDisplayed()值为true.
2、使用getAttribute("innerHTML")获取文本值
# 六位数支付密码输入自动输入问题
![image]()

>直接执行js脚本 selenium各种方法都试过了不行seleniumIDE里面的方法是type，但是selenium里面并没有提供type方法，执行sendkeys直接抛出异常，但下面这都js脚本可以执行成功，并解决这个问题。
````JavaScript 
String js="document.getElementById('newPaymentPassword_rsainput').value="+payPassword;
String js1="document.getElementById('newPaymentPasswordConfirm_rsainput').value="+payPassword;
runScript(js);
runScript(js1);
````

# 悬浮框处理问题
![image](http://024028.oss-cn-hangzhou-zmf.aliyuncs.com/uploads/cpyy/ProductQualityOperationTeam/fc20ea740843bc2781552c4779daf717/image.png)

>若想直接通过定位元素去执行element.click()进行对立即接入按钮的点击;此时会抛出异常报没有找到该element
此时需要点击该空间所在的div，然后再点击该按钮
下面是页面展示对应的html
````Html
<div id="J_mpProductDetailJoinWrap" class="mpProductDetailJoinWrap fixed">
  已有支付宝账号，
<a class="mpProductContractLink" href="/signing/steps.htm?productCode=I1011000290000001000" seed="J_mpProductDetailJoinWrap-mpProductContractLinkT1" smartracker="on">立即接入</a>
</div>
````
>对应的实现代码如下：
````Java
getDriver().findElement(By.id("J_mpProductDetailJoinWrap")).click();
getDriver().findElement(By.className("mpProductContractLink")).click();
````






