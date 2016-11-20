#FrescoUtil


1. 根据尺寸，url下载图片
```java

                ImageRequestBuilder builder = ImageRequestBuilder.newBuilderWithSource(Uri.parse(uri));
                if (width > 0 && height > 0) {
                    builder.setResizeOptions(new ResizeOptions(width, height));
                }
                ImageRequest imageRequest = builder.build();

                ImagePipeline imagePipeline = Fresco.getImagePipeline();
                DataSource<CloseableReference<CloseableImage>> dataSource = imagePipeline.fetchDecodedImage(imageRequest, context);

                dataSource.subscribe(new BaseBitmapDataSubscriber() {
                                         @Override
                                         public void onNewResultImpl(@Nullable Bitmap bitmap) {
                                            
                                         }

                                         @Override
                                         public void onFailureImpl(DataSource dataSource) {
                                             
                                         }
                                     },
                        UiThreadImmediateExecutorService.getInstance());

```

2. 获取缓存图片地址
```java

  public void getChachedAddr(View view) {
        ImageRequest request = ImageRequest.fromUri(url);
        CacheKey cacheKey = DefaultCacheKeyFactory.getInstance().getEncodedCacheKey(request, null);
        boolean hasCached = ImagePipelineFactory.getInstance().getMainFileCache().hasKey(cacheKey);
        if (hasCached) {
            BinaryResource binaryResource = ImagePipelineFactory.getInstance().getMainFileCache().getResource(cacheKey);
            File file = ((FileBinaryResource) binaryResource).getFile();
            if (file != null) {
                mChacheAddr.setText(file.getAbsolutePath());
            }
        }
    }

```