# Andorid-ImagesPickers
Andorid-ImagesPickers是一个图片选择（单选/多选）、拍照、裁剪的图片选择器。使用方便，功能可自己配置，Andorid-ImagesPickers自身并没有强制绑定某个ImageLoader，开发者可以根据自己项目给Andorid-ImagesPickers配置图片加载器。<br>

###[GitHub 项目地址](https://github.com/jaikydota/Andorid-ImagesPickers)

![截图](https://github.com/jaikydota/Andorid-ImagesPickers/blob/master/Demo/GIF.gif) 

[Download Apk](https://github.com/jaikydota/Andorid-ImagesPickers/blob/master/Demo/app-debug.apk)
 

## 问什么使用ImagesPickers
也许有人会问：系统不是有相册选择器吗，为什么还有做一个图片选择器呢，有必要吗？我告诉你很有必要。微信，QQ等等App它们都是自己带图片选择器，并没有直接调系统的图片选择器。为什么要这么做呢？我总结出一下几点，使用本图片选择器下面的问题你都不用考虑，就是这么的任性：
 * 最大的问题就是兼容性了，手机厂商那么多，相册软件那么多从而引起各种奇葩的问题
 * 有些手机拍照图片倒立情况（如三星和魅族）
 * 拿到的bitmap或uri为空
 * 非常频繁出现OOM
 * 不支持多选
 * 拍照/选择图片/裁剪视乎用起来有些麻烦，加上处理一些旋转、裁剪、压缩就更加麻烦了，代码多得不行不行的。
 * 系统的图片选择UI上与自己APP样式不统一
 * 有些不支持图片旋转
 * ....



## Demo展示

![Demo演示](https://github.com/jaikydota/Andorid-ImagesPickers/blob/master/Demo/GIF6.gif) 

 
## 使用说明

### 步骤一：

#### 通过Gradle抓取

```groovy
//只上传到了jcenter
allprojects {
    repositories {
        jcenter()
    }
}
dependencies {
    compile 'com.jaikydota.imagespickers:imagespickers:1.0.0'
}
```



### 步骤二：

在 `AndroidManifest.xml` 中 添加 如下权限

```xml
<!-- 从sdcard中读取数据的权限 -->
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
<!-- 往sdcard中写入数据的权限 -->
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />

```


### 步骤三：

#####创建 图片加载器 (其中可以按照 喜好  使用不同的 第三方图片加载框架 以下为Glide示例)

```java
public class GlideLoader implements com.yancy.imageselector.ImageLoader {

   @Override
   public void displayImage(Context context, String path, ImageView imageView) {
       Glide.with(context)
               .load(path)
               .placeholder(com.yancy.imageselector.R.mipmap.imageselector_photo)
               .centerCrop()
               .into(imageView);
   }

}

```    

### 步骤四：

#### 配置 `ImageConfig`

##### UI 视图配置

```java
 ImageConfig imageConfig
      = new ImageConfig.Builder(new GlideLoader())
     // 如果在 4.4 以上，则修改状态栏颜色 （默认黑色）
     .steepToolBarColor(getResources().getColor(R.color.blue))
     // 标题的背景颜色 （默认黑色）
     .titleBgColor(getResources().getColor(R.color.blue))
     // 提交按钮字体的颜色  （默认白色）
     .titleSubmitTextColor(getResources().getColor(R.color.white))
     // 标题颜色 （默认白色）
     .titleTextColor(getResources().getColor(R.color.white))
     .build();
```

##### 多选
```java
 ImageConfig imageConfig
        = new ImageConfig.Builder(new GlideLoader())
        .steepToolBarColor(getResources().getColor(R.color.blue))
        .titleBgColor(getResources().getColor(R.color.blue))
        .titleSubmitTextColor(getResources().getColor(R.color.white))
        .titleTextColor(getResources().getColor(R.color.white))
        // 开启多选   （默认为多选） 
        .mutiSelect()
        // 多选时的最大数量   （默认 9 张）
        .mutiSelectMaxSize(9)
        // 开启拍照功能 （默认关闭）
        .showCamera()
        // 已选择的图片路径
        .pathList(path)
        // 拍照后存放的图片路径（默认 /temp/picture） （会自动创建）
        .filePath("/ImageSelector/Pictures")
        .build();


ImageSelector.open(MainActivity.this, imageConfig);   // 开启图片选择器
```

##### 单选
```java
 ImageConfig imageConfig
        = new ImageConfig.Builder(new GlideLoader())
        .steepToolBarColor(getResources().getColor(R.color.blue))
        .titleBgColor(getResources().getColor(R.color.blue))
        .titleSubmitTextColor(getResources().getColor(R.color.white))
        .titleTextColor(getResources().getColor(R.color.white))
        // 开启单选   （默认为多选） 
        .singleSelect()
        // 开启拍照功能 （默认关闭）
        .showCamera()
        // 拍照后存放的图片路径（默认 /temp/picture） （会自动创建）
        .filePath("/ImageSelector/Pictures")
        .build();


ImageSelector.open(MainActivity.this, imageConfig);   // 开启图片选择器
```

##### 单选1：1 便捷截图
```java
 ImageConfig imageConfig
        = new ImageConfig.Builder(new GlideLoader())
        .steepToolBarColor(getResources().getColor(R.color.blue))
        .titleBgColor(getResources().getColor(R.color.blue))
        .titleSubmitTextColor(getResources().getColor(R.color.white))
        .titleTextColor(getResources().getColor(R.color.white))
        // (截图默认配置：关闭    比例 1：1    输出分辨率  500*500)
        .crop()  
        // 开启单选   （默认为多选） 
        .singleSelect()
        // 开启拍照功能 （默认关闭）
        .showCamera()
        // 拍照后存放的图片路径（默认 /temp/picture） （会自动创建）
        .filePath("/ImageSelector/Pictures")
        .build();


ImageSelector.open(MainActivity.this, imageConfig);   // 开启图片选择器
```

##### 单选自定义截图
```java
 ImageConfig imageConfig
        = new ImageConfig.Builder(new GlideLoader())
        .steepToolBarColor(getResources().getColor(R.color.blue))
        .titleBgColor(getResources().getColor(R.color.blue))
        .titleSubmitTextColor(getResources().getColor(R.color.white))
        .titleTextColor(getResources().getColor(R.color.white))
        // (截图默认配置：关闭    比例 1：1    输出分辨率  500*500)
        .crop(1, 2, 500, 1000) 
        // 开启单选   （默认为多选） 
        .singleSelect()
        // 开启拍照功能 （默认关闭）
        .showCamera()
        // 拍照后存放的图片路径（默认 /temp/picture） （会自动创建）
        .filePath("/ImageSelector/Pictures")
        .build();


ImageSelector.open(MainActivity.this, imageConfig);   // 开启图片选择器
```
### 步骤五：
 
在  `onActivityResult` 中获取选中的照片路径 数组 :
 
```java
@Override
protected void onActivityResult(int requestCode, int resultCode, Intent data) {
 super.onActivityResult(requestCode, resultCode, data);
  if (requestCode == ImageSelector.IMAGE_REQUEST_CODE && resultCode == RESULT_OK && data != null) {
  
    // Get Image Path List
     List<String> pathList = data.getStringArrayListExtra(ImageSelectorActivity.EXTRA_RESULT);

     for (String path : pathList) {
         Log.i("ImagePathList", path);
     }
  }
}
```

## 代码示例
```java
public class MainActivity extends AppCompatActivity {

    private Button btn1, btn2;
    private TextView tv1;
    private ArrayList<String> path = new ArrayList<>();

    public static final int REQUEST_CODE = 123;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        
        btn1 = (Button) findViewById(R.id.btn1);
        btn2 = (Button) findViewById(R.id.btn2);
        tv1 = (TextView) findViewById(R.id.tv1);
        //单选
        btn1.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                ImageConfig imageConfig = new ImageConfig.Builder(
                        new GlideLoader())
                        .steepToolBarColor(getResources().getColor(R.color.titleBlue))
                        .titleBgColor(getResources().getColor(R.color.titleBlue))
                        .titleSubmitTextColor(getResources().getColor(R.color.white))
                        .titleTextColor(getResources().getColor(R.color.white))
                        // 开启单选   （默认为多选）  (单选 为 singleSelect)
                        .singleSelect()
                        //.crop()
                        // 开启拍照功能 （默认开启）
                        //.showCamera()
                        .requestCode(REQUEST_CODE)
                        .build();
                ImageSelector.open(MainActivity.this, imageConfig);
            }
        });
        //多选
        btn2.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                ImageConfig imageConfig = new ImageConfig.Builder(
                        // GlideLoader 可用自己用的缓存库
                        new GlideLoader())
                        // 如果在 4.4 以上，则修改状态栏颜色 （默认黑色）
                        .steepToolBarColor(getResources().getColor(R.color.titleBlue))
                        // 标题的背景颜色 （默认黑色）
                        .titleBgColor(getResources().getColor(R.color.titleBlue))
                        // 提交按钮字体的颜色  （默认白色）
                        .titleSubmitTextColor(getResources().getColor(R.color.white))
                        // 标题颜色 （默认白色）
                        .titleTextColor(getResources().getColor(R.color.white))
                        // 开启多选   （默认为多选）  (单选 为 singleSelect)
                        //.singleSelect()
                        //裁剪
                        //.crop()
                        // 多选时的最大数量   （默认 9 张）
                        .mutiSelectMaxSize(9)
                        // 已选择的图片路径
                        .pathList(path)
                        // 拍照后存放的图片路径（默认 /temp/picture）
                        .filePath("/temp")
                        // 开启拍照功能 （默认开启）
                        .showCamera()
                        .requestCode(REQUEST_CODE)
                        .build();
                ImageSelector.open(MainActivity.this, imageConfig);
            }
        });
    }


    @Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        super.onActivityResult(requestCode, resultCode, data);
        if (requestCode == REQUEST_CODE && resultCode == RESULT_OK && data != null) {
            List<String> pathList = data.getStringArrayListExtra(ImageSelectorActivity.EXTRA_RESULT);

            tv1.setText("");
            for (String path : pathList) {
                tv1.append(path);
            }

            path.clear();
            path.addAll(pathList);
        }
    }
}

```

 
## 关于作者
* Email: [jaikydota@163.com](mailto:jaikydota@163.com)

