title: Android Material Design - Floating Labels for EditText
date: 2015-12-30 10:42:59
categories: [Android]
tags: [Material Design, EditText, Floating Lables]
---

>原文作者：Ravi Tamada
>原文地址：[http://www.androidhive.info/...aterial-design/](http://www.androidhive.info/2015/04/android-getting-started-with-material-design/)


Android浮动标签在android设计支持库有介绍，在EditText上显示一个悬浮的标签。最初它在EditText中作为字段为空时的一个提示。当用户开始输入文本，它通过一个动画的形式，移动到悬浮标签的位置。

这篇文章通过一个简单的表单验证的例子，演示了Floating Lables的用法。


源码下载地址：[http://download.androidhive.info/...dfyJJ0xyaJTvXugo0HTV0LBnG9w](http://download.androidhive.info/download?code=J5TnQr8DLL52kPlAxeIk9Z3H21tlAtFcD74lW1gWZwyM6aEBkra49p%2FxpDDZz5ZfPieGEGoAopEZQOxyUGNRKuXhmSxB%2FW6QlimXGOiu8gWcH1pqtQKfO5AfA%3D%3DV7JclQNddfyJJ0xyaJTvXugo0HTV0LBnG9w)
视频演示地址：[yutube-display](https://youtu.be/TYhpFJ58g6Y)


### TextInputLayout

在Material Design支持库中一个新的元素，叫作[TextInputLayout][1]，用于在EditText上展示悬浮标签。为了显示悬浮标签，EditText被TextInputLayout
所包裹。你也可以给EditText设置一个错误的信息，通过使用`setErrorEnabled()`和`setError()`方法。

TextInputLayout采用了EditText**android:hint**属性的值来作为悬浮标签显示。

``` xml
<android.support.design.widget.TextInputLayout
android:id="@+id/input_layout_password"
android:layout_width="match_parent"
android:layout_height="wrap_content">
 
        <EditText
            android:id="@+id/input_password"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:hint="@string/hint_email" />
 
</android.support.design.widget.TextInputLayout>
```

![android-design-support-library-floating-labels](http://www.androidhive.info/wp-content/uploads/2015/09/android-design-support-library-floating-labels.png)


### 简单的表单验证示例

现在让我们来创建一个简单的android应用，去真正的了解TextInputLayout的用法。这个应用包含了一个带有悬浮标签的简答表单，输入验证和错误信息启用。

**1.** 在Android Studio中，通过**File ⇒ New Project**并填好其它信息来新建一个项目。

**2.** 打开**build.gradle**并且添加Material Design支持库的依赖。

`com.android.support:design:23.0.1`

`build.gradle`
	
``` xml
dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    compile 'com.android.support:appcompat-v7:23.0.1'
    compile 'com.android.support:design:23.0.1'
}
```

**3.** 通过[这里][2]提到的步骤，应用Material Design主题，但这不是必须的。

**4.** 添加下面字符串到**res ⇒ values => strings.xml**下面。

`strings.xml`

``` xml
<resources>
    <string name="app_name">Floating Labels</string>
    <string name="action_settings">Settings</string>
    <string name="hint_name">Full Name</string>
    <string name="hint_email">Email</string>
    <string name="hint_password">Password</string>
    <string name="btn_sign_up">Sign Up</string>
    <string name="err_msg_name">Enter your full name</string>
    <string name="err_msg_email">Enter valid email address</string>
    <string name="err_msg_password">Enter the password</string>
</resources>
```

**5.** 打开主activity的**activity_main.xml**布局文件，然后添加如下代码。这些代码创建了一个简单的表单，有三个输入框。这里你可以看到EditText被TextInputLayout所包裹。

`activity_main.xml`

``` xml
<android.support.design.widget.CoordinatorLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
 
    <android.support.design.widget.AppBarLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar">
 
        <android.support.v7.widget.Toolbar
            android:id="@+id/toolbar"
            android:layout_width="match_parent"
            android:layout_height="?attr/actionBarSize"
            android:background="?attr/colorPrimary"
            app:layout_scrollFlags="scroll|enterAlways"
            app:popupTheme="@style/ThemeOverlay.AppCompat.Light" />
    </android.support.design.widget.AppBarLayout>
 
    <LinearLayout
        android:layout_width="fill_parent"
        android:layout_height="match_parent"
        android:layout_marginTop="?attr/actionBarSize"
        android:orientation="vertical"
        android:paddingLeft="20dp"
        android:paddingRight="20dp"
        android:paddingTop="60dp">
 
        <android.support.design.widget.TextInputLayout
            android:id="@+id/input_layout_name"
            android:layout_width="match_parent"
            android:layout_height="wrap_content">
 
            <EditText
                android:id="@+id/input_name"
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:singleLine="true"
                android:hint="@string/hint_name" />
        </android.support.design.widget.TextInputLayout>
 
        <android.support.design.widget.TextInputLayout
            android:id="@+id/input_layout_email"
            android:layout_width="match_parent"
            android:layout_height="wrap_content">
 
            <EditText
                android:id="@+id/input_email"
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:inputType="textEmailAddress"
                android:hint="@string/hint_email" />
        </android.support.design.widget.TextInputLayout>
 
        <android.support.design.widget.TextInputLayout
            android:id="@+id/input_layout_password"
            android:layout_width="match_parent"
            android:layout_height="wrap_content">
 
            <EditText
                android:id="@+id/input_password"
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:inputType="textPassword"
                android:hint="@string/hint_password" />
        </android.support.design.widget.TextInputLayout>
 
        <Button android:id="@+id/btn_signup"
            android:layout_width="fill_parent"
            android:layout_height="wrap_content"
            android:text="@string/btn_sign_up"
            android:background="@color/colorPrimary"
            android:layout_marginTop="40dp"
            android:textColor="@android:color/white"/>
 
    </LinearLayout>
 
</android.support.design.widget.CoordinatorLayout>
```

**6.** 打开**MainActivity.java**并且按照以下代码修改，这里我已经添加了一些方法去验证用户的输入数据比如名字、email、密码。我也. 我也指定了TextWatcher给所有的edittext来验证用户正在输入的内容，当输入无效或者为空时，setError()方法就会被调用来显示错误信息。

`MainActivity.java`

``` java
package info.androidhive.floatinglabels;
 
import android.os.Bundle;
import android.support.design.widget.TextInputLayout;
import android.support.v7.app.AppCompatActivity;
import android.support.v7.widget.Toolbar;
import android.text.Editable;
import android.text.TextUtils;
import android.text.TextWatcher;
import android.view.View;
import android.view.WindowManager;
import android.widget.Button;
import android.widget.EditText;
import android.widget.Toast;
 
public class MainActivity extends AppCompatActivity {
 
    private Toolbar toolbar;
    private EditText inputName, inputEmail, inputPassword;
    private TextInputLayout inputLayoutName, inputLayoutEmail, inputLayoutPassword;
    private Button btnSignUp;
 
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
 
        toolbar = (Toolbar) findViewById(R.id.toolbar);
        setSupportActionBar(toolbar);
 
        inputLayoutName = (TextInputLayout) findViewById(R.id.input_layout_name);
        inputLayoutEmail = (TextInputLayout) findViewById(R.id.input_layout_email);
        inputLayoutPassword = (TextInputLayout) findViewById(R.id.input_layout_password);
        inputName = (EditText) findViewById(R.id.input_name);
        inputEmail = (EditText) findViewById(R.id.input_email);
        inputPassword = (EditText) findViewById(R.id.input_password);
        btnSignUp = (Button) findViewById(R.id.btn_signup);
 
        inputName.addTextChangedListener(new MyTextWatcher(inputName));
        inputEmail.addTextChangedListener(new MyTextWatcher(inputEmail));
        inputPassword.addTextChangedListener(new MyTextWatcher(inputPassword));
 
        btnSignUp.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                submitForm();
            }
        });
    }
 
    /**
     * Validating form
     */
    private void submitForm() {
        if (!validateName()) {
            return;
        }
 
        if (!validateEmail()) {
            return;
        }
 
        if (!validatePassword()) {
            return;
        }
 
        Toast.makeText(getApplicationContext(), "Thank You!", Toast.LENGTH_SHORT).show();
    }
 
    private boolean validateName() {
        if (inputName.getText().toString().trim().isEmpty()) {
            inputLayoutName.setError(getString(R.string.err_msg_name));
            requestFocus(inputName);
            return false;
        } else {
            inputLayoutName.setErrorEnabled(false);
        }
 
        return true;
    }
 
    private boolean validateEmail() {
        String email = inputEmail.getText().toString().trim();
 
        if (email.isEmpty() || !isValidEmail(email)) {
            inputLayoutEmail.setError(getString(R.string.err_msg_email));
            requestFocus(inputEmail);
            return false;
        } else {
            inputLayoutEmail.setErrorEnabled(false);
        }
 
        return true;
    }
 
    private boolean validatePassword() {
        if (inputPassword.getText().toString().trim().isEmpty()) {
            inputLayoutPassword.setError(getString(R.string.err_msg_password));
            requestFocus(inputPassword);
            return false;
        } else {
            inputLayoutPassword.setErrorEnabled(false);
        }
 
        return true;
    }
 
    private static boolean isValidEmail(String email) {
        return !TextUtils.isEmpty(email) && android.util.Patterns.EMAIL_ADDRESS.matcher(email).matches();
    }
 
    private void requestFocus(View view) {
        if (view.requestFocus()) {
            getWindow().setSoftInputMode(WindowManager.LayoutParams.SOFT_INPUT_STATE_ALWAYS_VISIBLE);
        }
    }
 
    private class MyTextWatcher implements TextWatcher {
 
        private View view;
 
        private MyTextWatcher(View view) {
            this.view = view;
        }
 
        public void beforeTextChanged(CharSequence charSequence, int i, int i1, int i2) {
        }
 
        public void onTextChanged(CharSequence charSequence, int i, int i1, int i2) {
        }
 
        public void afterTextChanged(Editable editable) {
            switch (view.getId()) {
                case R.id.input_name:
                    validateName();
                    break;
                case R.id.input_email:
                    validateEmail();
                    break;
                case R.id.input_password:
                    validatePassword();
                    break;
            }
        }
    }
}
```

![android-material-design-floating-labels](http://www.androidhive.info/wp-content/uploads/2015/09/android-material-design-floating-labels.png)

![android-material-design-floating-labels-error-messages](http://www.androidhive.info/wp-content/uploads/2015/09/android-material-design-floating-labels-error-messages.png)



[1]: https://developer.android.com/intl/zh-tw/reference/android/support/design/widget/TextInputLayout.html
[2]: http://tips.androidhive.info/2015/09/android-how-to-apply-material-design-theme/