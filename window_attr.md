#Window属性 

```xml
 <item name="android:windowFrame">@mipmap/setup_</item>
```

官方说明
Drawable to use as a frame around the window.
[网址](https://developer.android.com/reference/android/R.attr.html#windowFrame)


每刷新window都会绘制此drawable不影响任何触摸事件
以下是滑动屏幕时的动画


![](images/window_frame.gif)


```xml
 <item name="android:windowContentOverlay">@mipmap/setup_</item>

```

官方说明
This Drawable is overlaid over the foreground of the Window's content area, usually to place a shadow below the title.
此drawable 用于绘制覆盖窗口内容区域，通常用于绘制title下面的阴影效果.[网址](https://developer.android.com/reference/android/R.attr.html#windowContentOverlay)
其实这跟windowFrame一样的，只是两个的绘制区域不一样,windowContentOverlay是不包括title区域


fitsSystemWindows  
Boolean internal attribute to adjust view layout based on system windows such as the status bar. If true, adjusts the padding of this view to leave space for the system windows. Will only take effect if this view is in a non-embedded activity.  
navigationBarColor  





了涉及到法律的手机发送的连接方式的理解

* WindowManager.LayoutParams.FLAG_SECURE
防止截屏 activity setContentView之前getWindow().addFlags(WindowManager.LayoutParams.FLAG_SECURE);
