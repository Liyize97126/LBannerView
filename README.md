## LBannerView 使用介绍

### 轮播控件千千万，为啥要自己再写一套？？？

#### 其一 是因为最近项目需求，小圆点再轮播图控件的底部，不是覆盖在图片上

#### 其二 也是项目需求，图片左右两边突出 类似效果一页看到左右突出（一页显示三张图，两边的图漏出一些图片）

#### 其三 就是这个框架了，哈哈

        
#### 自定义属性含义

        app:bottomColor="@color/transparent"   //底部遮罩 背景颜色
        
        app:dotGravity="center"         //小点位置  中间 左边 右边
       
        app:dotIndicatorFocus="@color/colorPrimary" // 当前选中点的颜色
       
        app:dotIndicatorNormal="@color/colorGray"   // 未选中的点的颜色
       
        app:widthProportion="8"                     //设置宽高比时 宽度比例值
       
        app:heightProportion="3"                    //设置宽高比时 高度比例值
       
        app:bulge="true"                            //是否时两边突出的模式
        
        app:bulgeDistance="30"                      // 两边显示突出的距离
        
        app:bottomType="belowPic"                   // 底部整体小点在轮播控件里面的位置，是底部覆盖在轮播图上，还是在轮播图下方
       
        app:dotSize="10dp"                          //小点之间的间距



### Dependency

#### Step 1. Add the JitPack repository to your build file
    
```
        allprojects {
                repositories {
                    ...
                    maven { url 'https://jitpack.io' }
                }
            }
```
#### Step 2. Add the dependency

```
    dependencies {
       	     implementation 'com.github.fazhongxu:LBannerView:v0.1.5'

	}
```



#### 布局文件使用

    <json.xxl.com.lbannerview.BannerView
        android:layout_marginTop="20dp"
        android:id="@+id/banner_view_below"
        android:layout_width="match_parent"
        android:layout_height="0dp"
        app:bottomColor="@color/transparent"
        app:dotGravity="center"
        app:dotIndicatorFocus="@color/colorPrimary"
        app:dotIndicatorNormal="@color/colorGray"
        app:widthProportion="8"
        app:heightProportion="3"
        app:bulge="true"
        app:bulgeDistance="30"
        app:bottomType="belowPic"
        app:dotSize="10dp" />
        
        
#### 代码里面使用

#### Java方式

```
   final BannerView bannerView = findViewById(R.id.banner_view);
        final BannerView belowBannerView = findViewById(R.id.banner_view_below);

        bannerView.setAdapter(new BannerAdapter() {
            @Override
            public View getView(int position, View convertView) {
                ImageView imageView = new ImageView(BannerActivity.this);
                Glide.with(BannerActivity.this).load(bannberPictures[position]).into(imageView);
                return imageView;
            }

            @Override
            public int getCount() {
                return bannberPictures.length;
            }
            @Override
            public String getBannerDes(int position) {
                return bannerDes[position];
            }
        });

        // 如果 布局文件里面设置了  app:widthProportion="8"  app:heightProportion="3"  宽高比 不是直接固定的高度
       /* bannerView.post(new Runnable() {   // post 方式
            @Override
            public void run() {
                bannerView.setAdapter(new BannerAdapter() {
                    @Override
                    public View getView(int position, View convertView) {
                        ImageView imageView = new ImageView(BannerActivity.this);
                        Glide.with(BannerActivity.this).load(bannberPictures[position]).into(imageView);
                        return imageView;
                    }

                    @Override
                    public int getCount() {
                        return bannberPictures.length;
                    }

                    @Override
                    public String getBannerDes(int position) {
                        return bannerDes[position];
                    }
                });
            }
        });
        */
        belowBannerView.post(new Runnable() {  // 如果布局里宽高比例没设置 是直接写死的高度 则不用post延时
            @Override
            public void run() {
                belowBannerView.setAdapter(new BannerAdapter() {   //指示点在banner图片下方 左右两边设置突出
                    @Override
                    public View getView(int position, View convertView) {
                        ImageView imageView = new ImageView(BannerActivity.this);
                        Glide.with(BannerActivity.this).load(bannberPictures[position]).into(imageView);
                        return imageView;
                    }

                    @Override
                    public int getCount() {
                        return bannberPictures.length;
                    }
                });
            }
        });
```


#### kotlin 方式

```
 banner_view.post {  // //设置了宽高比 所以需要post 否则拿不到宽度 显示不出图片 或则显示的图片高度不对
            kotlin.run {
                banner_view.setAdapter(object : BannerAdapter(){
                    override fun getView(position: Int, convertView: View?): View {
                        val imageView  = ImageView(this@MainActivity)
                        Glide.with(this@MainActivity)
                                .load(bannberPictures.get(position))
                                .into(imageView)
                        return imageView
                    }

                    override fun getCount(): Int {
                        return bannberPictures.size
                    }

                    override fun getBannerDes(position: Int): String {
                        return bannerDes.get(position)
                    }

                })
            }

        }

        banner_view_below.post{
            kotlin.run {
                banner_view_below.setAdapter(object : BannerAdapter() {
                    override fun getView(position: Int, convertView: View?): View {
                        val imageView = ImageView(this@MainActivity)
                        Glide.with(this@MainActivity)
                                .load(bannberPictures.get(position))
                                .into(imageView)
                        return imageView
                    }
                    override fun getCount(): Int {
                        return bannberPictures.size
                    }

                })
            }
        }
```

### 效果图


<img src="https://github.com/fazhongxu/IBannerView/blob/master/images/screenshot.png" width="80%" height="30%">


<img src="https://github.com/fazhongxu/IBannerView/blob/master/images/screenshot_below.png" width="50%" height="50%" margin:0 auto>

