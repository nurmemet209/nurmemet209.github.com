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