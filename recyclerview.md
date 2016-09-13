#关于RecyclerView你应该知道的一切


###常用函数

* scrollToPosition
函数原型
```java

/**
     * <p>Scroll the RecyclerView to make the position visible.</p>
     *滚动RecyclerView到让position出于可见状态位置，意思是如果position是在第一个可见View的上面,让position
     *滚动到第一个可见View的位置，如果position是在最后一个可见View的下面，让position滚动到
     *最后一个可见View的位置，如果position出于可见状态不做任何处理
     * <p>RecyclerView will scroll the minimum amount that is necessary to make the
     * target position visible. If you are looking for a similar behavior to
     * {@link android.widget.ListView#setSelection(int)} or
     * {@link android.widget.ListView#setSelectionFromTop(int, int)}, use
     * {@link #scrollToPositionWithOffset(int, int)}.</p>
     *
     * <p>Note that scroll position change will not be reflected until the next layout call.</p>
     *
     * @param position Scroll to this adapter position
     * @see #scrollToPositionWithOffset(int, int)
     */
    @Override
    public void scrollToPosition(int position) 
```

* scrollToPositionWithOffset
函数原型
```java
/**
     * Scroll to the specified adapter position with the given offset from resolved layout
     * start. Resolved layout start depends on {@link #getReverseLayout()},
     * 滚动到让position出于可见状态位置，并且position 离布局开始坐标（默认是RecyclerView的0,0位置，如果是反向布局  
     *  RecyclerView的底部 ）的距离为offset
     * {@link ViewCompat#getLayoutDirection(android.view.View)} and {@link #getStackFromEnd()}.
     * <p>
     * For example, if layout is {@link #VERTICAL} and {@link #getStackFromEnd()} is true, calling
     * <code>scrollToPositionWithOffset(10, 20)</code> will layout such that
     * <code>item[10]</code>'s bottom is 20 pixels above the RecyclerView's bottom.
     * <p>
     * Note that scroll position change will not be reflected until the next layout call.
     * <p>
     * If you are just trying to make a position visible, use {@link #scrollToPosition(int)}.
     * 如果你只是想让position出于可见状态请使用scrollToPosition
     * @param position Index (starting at 0) of the reference item.
     * @param offset   The distance (in pixels) between the start edge of the item view and
     *                 start edge of the RecyclerView.
     * @see #setReverseLayout(boolean)
     * @see #scrollToPosition(int)
     */
    public void scrollToPositionWithOffset(int position, int offset)
```