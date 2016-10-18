#android webview


1. 网页跳app
```xml
 <activity android:name=".MainActivity">
            <intent-filter>
                <action android:name="android.intent.action.MAIN"/>
                <category android:name="android.intent.category.LAUNCHER"/>
            </intent-filter>
            <intent-filter>
                <action android:name="android.intent.action.VIEW"/>
                <category android:name="android.intent.category.BROWSABLE"/>
                <category android:name="android.intent.category.DEFAULT"/>

                <data
                    android:host="index"
                    android:scheme="pppcar"
                    ></data>
            </intent-filter>
        </activity>
```
intent到底发给哪个activity，需要进行三个匹配，一个是action，一个是category，一个是data。
理论上来说，如果intent不指定category，那么无论intent filter的内容是什么都应该是匹配的。但是，如果是implicit intent，Android默认给加上一个CATEGORY_DEFAULT，这样的话如果intent filter中没有android.intent.category.DEFAULT这个category的话，匹配测试就会失败。所以，如果你的 activity支持接收implicit intent的话就一定要在intent filter中加入android.intent.category.DEFAULT。
```html

<html>

<head>
    <title>我的第一个 HTML 页面</title>
</head>

<body>
<p>body 元素的内容会显示在浏览器中。</p>
<p>title 元素的内容会显示在浏览器的标题栏中。</p>
<a href="pppcar://index">跳到app</a>
</body>

</html>


```

只能从浏览器到app跳转，不能从自己的app里的webview里打开对应的acitivity

2. shouldOverrideUrlLoading
如果地址包含#此函数无法拦截

3. 页面Js出错会导致白屏