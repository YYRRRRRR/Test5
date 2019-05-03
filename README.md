# Test5
1.写好输入网址的布局页面
```
activity_main.xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"

    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context="com.example.martinzou.android_exp_6.MainActivity">

   <LinearLayout
       android:layout_width="match_parent"
       android:layout_height="wrap_content"
       android:orientation="vertical">
       <EditText
           android:id="@+id/editText"
           android:layout_width="match_parent"
           android:layout_height="wrap_content"
           android:layout_marginBottom="8dp"
           android:layout_marginEnd="8dp"
           android:layout_marginStart="8dp"
           android:layout_marginTop="8dp"
           android:inputType="textPersonName"
           android:hint="输入网址"
           app:layout_constraintBottom_toTopOf="@+id/button"
           app:layout_constraintEnd_toEndOf="parent"
           app:layout_constraintHorizontal_bias="0.497"
           app:layout_constraintStart_toStartOf="parent"
           app:layout_constraintTop_toTopOf="parent"
           app:layout_constraintVertical_bias="0.788" />
    <Button
        android:id="@+id/button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginBottom="288dp"
        android:layout_marginEnd="8dp"
        android:layout_marginStart="8dp"
        android:text="@string/browser"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.498"
        app:layout_constraintStart_toStartOf="parent" />
   </LinearLayout>

</LinearLayout>

```
```
webview.xml
<?xml version="1.0" encoding="utf-8"?>
<android.support.constraint.ConstraintLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <WebView
        android:id="@+id/webview"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent">
    </WebView>

</android.support.constraint.ConstraintLayout>
```
定义两个活动
```
MainActivity.java
package com.example.martinzou.android_exp_6;

import android.content.Intent;
import android.net.Uri;
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;

public class MainActivity extends AppCompatActivity {
    private Button button;
    private EditText editText;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        button=(Button)findViewById(R.id.button);
        editText=(EditText)findViewById(R.id.editText) ;
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent=new Intent("com.action.webview");
                String url="http://"+editText.getText().toString();
                intent.addCategory("com.action.webviewcategory");
                intent.putExtra("url",url);
                startActivity(intent);
            }
        });
    }
}

```
```
WebViewActivity.java
package com.example.martinzou.android_exp_6;

import android.os.Bundle;
import android.support.annotation.Nullable;
import android.support.v7.app.AppCompatActivity;
import android.webkit.WebSettings;
import android.webkit.WebView;
import android.webkit.WebViewClient;

public class WebViewActivity extends AppCompatActivity{
    private WebView webView;
    @Override
    protected void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.webview);
        webView=(WebView)findViewById(R.id.webview);

        //url="http://www.baidu.com";浏览百度网页
        String url=getIntent().getExtras().getString("url");
        webView.loadUrl(url);
        // 覆盖WebView默认通过第三方或者是系统浏览器打开网页的行为，使得网页可以在WebVIew中打开
        webView.setWebViewClient(new WebViewClient(){

            @Override
            public boolean shouldOverrideUrlLoading(WebView view, String url) {
                // TODO Auto-generated method stub
                //返回值是true的时候控制网页在WebView中去打开，如果为false调用系统浏览器或第三方浏览器去打开
                view.loadUrl(url);
                return true;
            }
            //WebViewClient帮助WebView去处理一些页面控制和请求通知

        });
        //启用支持JavaScript
        WebSettings settings = webView.getSettings();
        settings.setJavaScriptEnabled(true);
        //WebView加载页面优先使用缓存加载
        settings.setCacheMode(WebSettings.LOAD_CACHE_ELSE_NETWORK);
    }
}

```
结果截图:
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190503111637409.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ2MDUz,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190503111720807.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwOTQ2MDUz,size_16,color_FFFFFF,t_70)
