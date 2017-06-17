## Android 按钮上的文本全是大写，解决办法
### 解决方法一，单个按钮修改
布局文件里，设置按钮 textAllCaps为false
```
<Button
        android:id="@+id/button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:onClick="onMediaExtractorTest"
        android:text="TestMediaExtractor"
        tools:layout_editor_absoluteX="33dp"
        tools:layout_editor_absoluteY="41dp"
        android:textAllCaps="false" />
```
注：设置后无效的原因 **android**:textAllCaps，前缀不要写错了（错误的写了 tools:textAllCaps造成设置无效）。

### 解决方法二，修改应用的主题样式
找到应用的主题样式： 
在 AndroiManifest.xml 的 <application> 标签的属性 android:theme 指定了应用的主题样式。 

打开主题样式文件，自定义一个 button 样式，并应用到主题里：
```
   <!-- 自定义按钮样式 -->
   <style name="MyCustomButton" parent="Widget.AppCompat.Button">
        <item name="android:textAllCaps">false</item>
   </style>

    <!-- Base application theme. -->
    <style name="AppTheme" parent="Theme.AppCompat.Light.DarkActionBar">
        <!-- Customize your theme here. -->
        <item name="colorPrimary">@color/colorPrimary</item>
        <item name="colorPrimaryDark">@color/colorPrimaryDark</item>
        <item name="colorAccent">@color/colorAccent</item>
        <!-- 把自定义样式添加到主题里 -->
        <item name="android:buttonStyle">@style/Button</item>
    </style>
```
注意：这种方法，在 android 4.4 上不生效。原因暂时不明。
## Android 按钮上的文本全是大写，原因

### 参考下材料设计原则
Button text is automatically changed to ALLCAPS in material design.  
https://material.io/guidelines/components/buttons.html#

### 查看Button源码
使用的编译SDK版本是 25，对应的 android.widget.Button 构造方法
```
    public Button(Context context) {
        this(context, null);
    }

    public Button(Context context, AttributeSet attrs) {
        this(context, attrs, com.android.internal.R.attr.buttonStyle);
    }

    public Button(Context context, AttributeSet attrs, int defStyleAttr) {
        this(context, attrs, defStyleAttr, 0);
    }

    public Button(Context context, AttributeSet attrs, int defStyleAttr, int defStyleRes) {
        super(context, attrs, defStyleAttr, defStyleRes);
    }
```
从应用的主题样式查起
```
Theme.AppCompat.Light.DarkActionBar 继承至
Base.Theme.AppCompat.Light.DarkActionBar 继承至
Base.Theme.AppCompat.Light 继承至
Base.V7.Theme.AppCompat.Light 样式定义了按钮样式
<item name="buttonStyle">@style/Widget.AppCompat.Button</item>
```
根据按钮样式在向上查
```
Widget.AppCompat.Button
Base.Widget.AppCompat.Button
    <style name="Base.Widget.AppCompat.Button" parent="android:Widget">
        <item name="android:background">@drawable/abc_btn_default_mtrl_shape</item>
        <item name="android:textAppearance">?android:attr/textAppearanceButton</item>
        <item name="android:minHeight">48dip</item>
        <item name="android:minWidth">88dip</item>
        <item name="android:focusable">true</item>
        <item name="android:clickable">true</item>
        <item name="android:gravity">center_vertical|center_horizontal</item>
    </style>
```
至此，知道这个主题样式，并没有设置 android:textAllCaps 属性
### Android 框架代码
https://github.com/android/platform_frameworks_base/blob/master/core/res/res/values/attrs.xml
```
<!-- Present the text in ALL CAPS. This may use a small-caps form when available. -->
<attr name="textAllCaps" format="boolean" />
```
https://android.googlesource.com/platform/frameworks/base/+/master/core/res/res/values/styles_material.xml
```
    <style name="TextAppearance.Material.Button">
        <item name="textSize">@dimen/text_size_button_material</item>
        <item name="fontFamily">@string/font_family_button_material</item>
        <item name="textAllCaps">true</item>
        <item name="textColor">?attr/textColorPrimary</item>
    </style>
```

## 相关问题
* 在应用中设置统一的按钮风格： R.attr.buttonStyle