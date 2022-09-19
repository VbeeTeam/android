# Activity组件

- （1）活动 （Activity)

- （2）服务 （Service ）

- （3）广播接收器 （BroadcastReceiver ） 

- （4）内容提供者 （Content Provider ）

## **Activity** **的启动和结束**

new 》module》study03

###### 从当前页面跳到新页面，跳转代码如下： 

###### Intent对象意图

-  startActivity(new Intent(源页面.this, 目标页面.class)); 

###### 从当前页面回到上一个页面，相当于关闭当前页面，返回代码如下： 

-  finish(); // 结束当前的活动页面

```xml
new > activity > StartActivity 、 FinishActivity
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:gravity="center">
    <Button
        android:id="@+id/btn_next"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="跳转下个页面"/>
</LinearLayout>
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">
    <ImageView
        android:id="@+id/iv_back"
        android:layout_width="40dp"
        android:layout_height="40dp"
        android:src="@drawable/ic_back"/>
    <Button
        android:id="@+id/btn_back"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="返回"/>
</LinearLayout>
```

```java
public class StartActivity extends AppCompatActivity implements View.OnClickListener {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_start);
//        Alt + Enter + Make....
        findViewById(R.id.btn_next).setOnClickListener(this);

    }
    @Override
    public void onClick(View v) {
        startActivity(new Intent(this, FinishActivity.class));
    }
}
public class FinishActivity extends AppCompatActivity implements View.OnClickListener {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_finish);
        findViewById(R.id.iv_back).setOnClickListener(this);
        findViewById(R.id.btn_back).setOnClickListener(this);
    }
    @Override
    public void onClick(View v) {
        if (v.getId() == R.id.iv_back || v.getId() == R.id.btn_back) {
            // 结束当前的活动页面
            finish();
        }
    }
}
```

## **Activity** **的生命周期**

![image-20220914222433286](C:\Users\15596\AppData\Roaming\Typora\typora-user-images\image-20220914222433286.png)

- onCreate：创建活动。把页面布局加载进内存，进入了初始状态。 

-  onStart：开始活动。把活动页面显示在屏幕上，进入了就绪状态。 

-  onResume：恢复活动。活动页面进入活跃状态，能够与用户正常交互，例如允许响应用户的点击动作、允许用户输入文字等等。 

-  onPause：暂停活动。页面进入暂停状态，无法与用户正常交互。 

-  onStop：停止活动。页面将不在屏幕上显示。 

-  onDestroy：销毁活动。回收活动占用的系统资源，把页面从内存中清除。 

-  onRestart：重启活动。重新加载内存中的页面数据。 

-  onNewIntent：重用已有的活动实例

```java
public class StartActivity extends AppCompatActivity implements View.OnClickListener {
    private static final String TAG = "test";
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
//        创建活动。把页面布局加载进内存，进入了初始状态。
        Log.d(TAG, "---StartActivity--- onCreate");
        setContentView(R.layout.activity_start);
//        Alt + Enter + Make....
        findViewById(R.id.btn_next).setOnClickListener(this);

    }

    @Override
    public void onClick(View v) {
        startActivity(new Intent(this, FinishActivity.class));
    }

    @Override
    protected void onStart() {
        super.onStart();
//        开始活动。把活动页面显示在屏幕上，进入了就绪状态。
        Log.d(TAG, "ActStartActivity onStart");
    }

    @Override
    protected void onResume() {
        super.onResume();
//        恢复活动。活动页面进入活跃状态，能够与用户正常交互，例如允许响应用户的点击动作、允许用户输入文字等等。
        Log.d(TAG, "ActStartActivity onResume");
    }

    @Override
    protected void onPause() {
        super.onPause();
//        暂停活动。页面进入暂停状态，无法与用户正常交互
        Log.d(TAG, "ActStartActivity onPause");
    }

    @Override
    protected void onStop() {
        super.onStop();
//        停止活动。页面将不在屏幕上显示。
        Log.d(TAG, "ActStartActivity onStop");
    }

    @Override
    protected void onDestroy() {
        super.onDestroy();
//        系统物理返回
//        销毁活动。回收活动占用的系统资源，把页面从内存中清除。
        Log.d(TAG, "ActStartActivity onDestroy");
    }

    @Override
    protected void onRestart() {
        super.onRestart();
//        重启活动。重新加载内存中的页面数据。
        Log.d(TAG, "ActStartActivity onRestart");
    }
}
```

### **各状态之间的切换过程** 

###### 打开新页面的方法调用顺序为： 

-  onCreate→onStart→onResume 

###### 关闭旧页面的方法调用顺序为： 

-  onPause→onStop→onDestroy 

## **Activity** **的启动模式** 

###### 某App先后打开两个活动，此时活动栈的变动情况如下图所示

![image-20220914233405241](C:\Users\15596\AppData\Roaming\Typora\typora-user-images\image-20220914233405241.png)

###### 依次结束已打开的两个活动，此时活动栈的变动情况如下图所示

![image-20220914233410165](C:\Users\15596\AppData\Roaming\Typora\typora-user-images\image-20220914233410165.png)

### **在配置文件中指定启动模式**

###### launchMode属性的取值说明见下表

![image-20220914233514038](C:\Users\15596\AppData\Roaming\Typora\typora-user-images\image-20220914233514038.png)

#### **默认启动模式** **standard**

该模式可以被设定，不在 manifest 设定时候，Activity 的默认模式就是 standard。在该模式下，启动的 Activity 会依照启动顺序被依次压入 Task 栈中：

![image-20220914234304847](C:\Users\15596\AppData\Roaming\Typora\typora-user-images\image-20220914234304847.png)

#### **栈顶复用模式** **singleTop**

在该模式下，如果栈顶 Activity 为我们要新建的 Activity（目标Activity），那么就不会重复创建新的Activity。

![image-20220914234340764](C:\Users\15596\AppData\Roaming\Typora\typora-user-images\image-20220914234340764.png)

**应用场景**

适合开启渠道多、多应用开启调用的 Activity，通过这种设置可以避免已经创建过的 Activity 被重复创建，多数通过动态设置使用

#### **栈内复用模式** **singleTask**

与 singleTop 模式相似，只不过 singleTop 模式是只是针对栈顶的元素，而 singleTask 模式下，如果task 栈内存在目标 Activity 实例，则将 task 内的对应 Activity 实例之上的所有 Activity 弹出栈，并将对应 Activity 置于栈顶，获得焦点。

![image-20220914234424615](C:\Users\15596\AppData\Roaming\Typora\typora-user-images\image-20220914234424615.png)

**应用场景**

**程序主界面**：我们肯定不希望主界面被创建多次，而且在主界面退出的时候退出整个 App 是最好的效果。

**耗费系统资源的****Activity**：对于那些及其耗费系统资源的 Activity，我们可以考虑将其设为 singleTask模式，减少资源耗费。

#### **全局唯一模式** **singleInstance**

在该模式下，我们会为目标 Activity 创建一个新的 Task 栈，将目标 Activity 放入新的 Task，并让目标Activity获得焦点。新的 Task 有且只有这一个 Activity 实例。 如果已经创建过目标 Activity 实例，则不会创建新的 Task，而是将以前创建过的 Activity 唤醒

![image-20220914234527329](C:\Users\15596\AppData\Roaming\Typora\typora-user-images\image-20220914234527329.png)

#### **动态设置启动模式**

在上述所有情况，都是我们在Manifest中通过 launchMode 属性设置的，这个被称为静态设置，动态设置是通过 Java 代码设置的。

通过 Intent 动态设置 Activity启动模式

```java
intent.setFlags();
```

如果同时有动态和静态设置，那么动态的优先级更高。接下来我们来看一下如何动态的设置 Activity 启动模式。

#### **在代码里面设置启动标志**

###### 启动标志的取值说明如下： 

-  Intent.FLAG_ACTIVITY_NEW_TASK：开辟一个新的任务栈 

-  Intent.FLAG_ACTIVITY_SINGLE_TOP：当栈顶为待跳转的活动实例之时，则重用栈顶的实例 

-  Intent.FLAG_ACTIVITY_CLEAR_TOP：当栈中存在待跳转的活动实例时，则重新创建一个新实例， 并清除原实例上方的所有实例 

-  Intent.FLAG_ACTIVITY_NO_HISTORY：栈中不保存新启动的活动实例 

-  Intent.FLAG_ACTIVITY_CLEAR_TASK：跳转到新页面时，栈中的原有实例都被清空 

### 常见场景

#### 不返回重复Activity

new > activity > ModeFirstActivity、ModeTwoActivity

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:gravity="center">
    <Button
        android:id="@+id/btn_next"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="跳转第二个页面"/>
</LinearLayout>
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:gravity="center">
    <Button
        android:id="@+id/btn_next"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="跳转第一个页面"/>
</LinearLayout>
```

```java
public class ModeFirstActivity extends AppCompatActivity implements View.OnClickListener {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_mode_first);
        //        Alt + Enter + Make....
        findViewById(R.id.btn_next).setOnClickListener(this);
    }
    @Override
    public void onClick(View v) {
        // 创建一个意图对象，准备跳到指定的活动页面
        Intent intent = new Intent(this, ModeTwoActivity.class);
        // 栈中存在待跳转的活动实例时，则重新创建该活动的实例，并清除原实例上方的所有实例
        intent.setFlags(Intent.FLAG_ACTIVITY_CLEAR_TOP);
        startActivity(intent);
    }
}
public class ModeTwoActivity extends AppCompatActivity implements View.OnClickListener {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_mode_two);
        //        Alt + Enter + Make....
        findViewById(R.id.btn_next).setOnClickListener(this);
    }

    @Override
    public void onClick(View v) {
        // 创建一个意图对象，准备跳到指定的活动页面
        Intent intent = new Intent(this, ModeFirstActivity.class);
        // 栈中存在待跳转的活动实例时，则重新创建该活动的实例，并清除原实例上方的所有实例
        intent.setFlags(Intent.FLAG_ACTIVITY_CLEAR_TOP);
        startActivity(intent);
    }
}
```

#### 登录不返回登录页

new > activity > LoginInputActivity、LoginSuccessActivity

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">
    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="这里是登录验证页面，此处省略了用户名和密码等输入框" />
    <Button
        android:id="@+id/btn_jump_success"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:gravity="center"
        android:text="跳到登录成功页面" />
</LinearLayout>
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">
    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="这里是登录成功页面，登录成功之后不必返回登录验证页面。请按返回键观察看看" />
    <Button
        android:id="@+id/btn_back"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="返回"/>
</LinearLayout>
```

```java
public class LoginInputActivity extends AppCompatActivity implements View.OnClickListener {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_login_input);
        findViewById(R.id.btn_jump_success).setOnClickListener(this);
    }

    @Override
    public void onClick(View v) {
        // 创建一个意图对象，准备跳到指定的活动页面
        Intent intent = new Intent(this, LoginSuccess.class);
        // 设置启动标志：跳转到新页面时，栈中的原有实例都被清空，同时开辟新任务的活动栈
        intent.setFlags(Intent.FLAG_ACTIVITY_CLEAR_TASK | Intent.FLAG_ACTIVITY_NEW_TASK);
        startActivity(intent);
    }
}
public class LoginSuccess extends AppCompatActivity implements View.OnClickListener {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_login_success);
        findViewById(R.id.btn_back).setOnClickListener(this);
    }

    @Override
    public void onClick(View v) {
        if (v.getId() == R.id.btn_back) {
            // 结束当前的活动页面
            finish();
        }
    }
}
```

### **显式****Intent****和隐式****Intent**

###### Intent是各个组件之间信息沟通的桥梁，它用于Android各组件之间的通信，主要完成下 列工作： 

-  标明本次通信请求从哪里来、到哪里去、要怎么走。 

-  发起方携带本次通信需要的数据内容，接收方从收到的意图中解析数据。 

-  发起方若想判断接收方的处理结果，意图就要负责让接收方传回应答的数据内容。

###### **Intent****的组成部分** 

![image-20220919204855836](C:\Users\15596\AppData\Roaming\Typora\typora-user-images\image-20220919204855836.png)

#### **显式****Intent**

###### 显式Intent，直接指定来源活动与目标活动，属于精确匹配。它有三种构建方式： 

-  在Intent的构造函数中指定。 

-  调用意图对象的setClass方法指定。 

-  调用意图对象的setComponent方法指定。 

```java
StartActivity
@Override
    public void onClick(View v) {
//        1.在Intent的构造函数中指定
//        Intent intent = new Intent(this, FinishActivity.class);
//        2.调用意图对象的setClass方法指定
        Intent intent = new Intent(); // 创建一个新意图
//        // 设置意图要跳转的目标活动
//        intent.setClass(this, FinishActivity.class);
//        3.调用意图对象的setComponent方法指定
        // 创建包含目标活动在内的组件名称对象
        ComponentName component = new ComponentName(this, FinishActivity.class);
        intent.setComponent(component); // 设置意图携带的组件信息
        startActivity(intent);
    }
```

#### **隐式****Intent** 

###### 隐式Intent，没有明确指定要跳转的目标活动，只给出一个动作字符串让系统自动匹配， 属于模糊匹配。动作名称既可以通过setAction方法指定，也可以通过构造函数Intent(String action)直接生成意图对象。常见的系统动作如下表：

![image-20220919205028320](C:\Users\15596\AppData\Roaming\Typora\typora-user-images\image-20220919205028320.png)

new > activity > ActionActivity

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">
    <Button
        android:id="@+id/btn_dial"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="跳转到拨号页面"/>
    <Button
        android:id="@+id/btn_sms"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="跳转到发短信页面"/>
    <Button
        android:id="@+id/btn_study02"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="跳转到另一个应用"/>
</LinearLayout>
```

```java
public class ActionUriActivity extends AppCompatActivity implements View.OnClickListener {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_action_uri);
        findViewById(R.id.btn_dial).setOnClickListener(this);
        findViewById(R.id.btn_sms).setOnClickListener(this);
        findViewById(R.id.btn_study02).setOnClickListener(this);
    }

    @Override
    public void onClick(View v) {
        String phoneNumber = "10086";
        Intent intent = new Intent();
        switch (v.getId()){
            case R.id.btn_dial:

//                设置意图动作准备拨号
                intent.setAction(Intent.ACTION_DIAL);
//                声明一个拨号的Uri
                Uri uri = Uri.parse("tel:" + phoneNumber);
                intent.setData(uri);
                startActivity(intent);
                break;
            case R.id.btn_sms:
//                设置意图动作准备发短信
                intent.setAction(Intent.ACTION_SENDTO);
//                声明一个拨号的Uri
                Uri uri2 = Uri.parse("smsto:" + phoneNumber);
                intent.setData(uri2);
                startActivity(intent);
                break;
            case R.id.btn_study02:
//                设置意图动作准备跳转其他应用
                intent.setAction("android.intent.action.CALCULATOR");
                intent.addCategory(Intent.CATEGORY_DEFAULT);
                startActivity(intent);
                break;
        }
    }
}
```

```xml
study02 > AndroidManifest.xml
<activity
            android:name=".CalculatorActivity"
<!--            exported:允许其他应用访问 -->
            android:exported="true">
<!--            桌面点图标启动-->
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
<!--            自定义其他打开启动方式-->
            <intent-filter>
                <action android:name="android.intent.action.CALCULATOR" />
                <category android:name="android.intent.category.DEFAULT" />
            </intent-filter>
        </activity>
```

### **向下一个****Activity****发送数据** 

###### Intent使用Bundle对象存放待传递的数据信息。 

###### Bundle对象操作各类型数据的读写方法说明见下表。

![image-20220919214214123](C:\Users\15596\AppData\Roaming\Typora\typora-user-images\image-20220919214214123.png)

**Bundle** 

- 在代码中发送消息包裹，调用意图对象的putExtras方法，即可存入消息包裹。 

- 在代码中接收消息包裹，调用意图对象的getExtras方法，即可取出消息包裹。 

new > activity > SendActivity | ReceiveActivity

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">
    <TextView
        android:id="@+id/tv_msg"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="我是一个消息"
        android:textSize="25sp"/>
    <Button
        android:id="@+id/btn_send"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="发送以上消息"/>
</LinearLayout>
```

```xml
public class SendActivity extends AppCompatActivity implements View.OnClickListener {
    private TextView tv_msg;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_send);
        tv_msg = findViewById(R.id.tv_msg);
        findViewById(R.id.btn_send).setOnClickListener(this);
    }
    @Override
    public void onClick(View v) {
       Intent intent = new Intent(this, ReceiveActivity.class);
//       创建一个新的包裹
       Bundle bundle = new Bundle();
       bundle.putString("time", Utils.getNowTime());
       bundle.putString("content", tv_msg.getText().toString());
//       包裹放入意图对象
       intent.putExtras(bundle);
       startActivity(intent);
    }
}
```

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">
    <TextView
        android:id="@+id/tv_receive"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="接收包裹"
        android:textSize="25sp"/>
</LinearLayout>
```

```java
public class ReceiveActivity extends AppCompatActivity {
    private TextView tv_receive;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_receive);

        tv_receive = findViewById(R.id.tv_receive);
//        从上一个页面传来的意图中获取快递包裹
        Bundle bundle = getIntent().getExtras();
        String time = bundle.getString("time");
        String content = bundle.getString("content");
        String desc = String.format("收到消息:\n请求时间:%s\n请求内容:%s", time,content);
        tv_receive.setText(desc);
    }
}
```

### **向上一个****Activity****返回数据**

###### 处理下一个页面的应答数据，详细步骤说明如下： 

-  上一个页面打包好请求数据，调用startActivityForResult方法执行跳转动作 

-  下一个页面接收并解析请求数据，进行相应处理 

-  下一个页面在返回上一个页面时，打包应答数据并调用setResult方法返回数据包裹 

-  上一个页面重写方法onActivityResult，解析获得下一个页面的返回数据 

new > activity > RequestActivity | ResponseActivity

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">
    <TextView
        android:id="@+id/tv_request"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="发送消息"
        android:textSize="25sp"/>
    <Button
        android:id="@+id/btn_request"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="传送请求数据"/>
    <TextView
        android:id="@+id/tv_response"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="返回结果显示"
        android:textSize="25sp"/>
</LinearLayout>
```

```java
public class RequestActivity extends AppCompatActivity implements View.OnClickListener {
    private TextView tv_request;
    private TextView tv_response;
    private ActivityResultLauncher<Intent> register;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_request);

        tv_request = findViewById(R.id.tv_request);
        tv_response = findViewById(R.id.tv_response);
        findViewById(R.id.btn_request).setOnClickListener(this);
//        注册一个接收返回参数结果接收对象
        register = registerForActivityResult(new ActivityResultContracts.StartActivityForResult(), new ActivityResultCallback<ActivityResult>() {
            @Override
            public void onActivityResult(ActivityResult result) {
                if( result != null){
                    Intent intent = result.getData();
                    if(intent != null && result.getResultCode() == Activity.RESULT_OK){
                        Bundle bundle = intent.getExtras();
                        String time = bundle.getString("time");
                        String content = bundle.getString("content");
                        String desc = String.format("收到返回消息:\n返回时间:%s\n返回内容:%s", time,content);
                        tv_response.setText(desc);
                    }
                }
            }
        });
//        }).var => ActivityResultLauncher<Intent> register =
    }
    @Override
    public void onClick(View v) {
        Intent intent = new Intent(this, ResponseActivity.class);
//       创建一个新的包裹
        Bundle bundle = new Bundle();
        bundle.putString("time", Utils.getNowTime());
        bundle.putString("content", tv_request.getText().toString());
//       包裹放入意图对象
        intent.putExtras(bundle);
//        startActivityForResult 过时了
        register.launch(intent);
    }
}
```

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">
    <TextView
        android:id="@+id/tv_request"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="显示接收消息"
        android:textSize="25sp"/>
    <Button
        android:id="@+id/btn_response"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="返回数据"/>
    <TextView
        android:id="@+id/tv_response"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="带回去的数据"
        android:textSize="25sp"/>
</LinearLayout>
```

```java
public class ResponseActivity extends AppCompatActivity implements View.OnClickListener {
    private TextView tv_response;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_response);

        TextView tv_request = findViewById(R.id.tv_request);
        tv_response = findViewById(R.id.tv_response);
//        获取从上一个页面传递过来的数据显示
        Bundle bundle = getIntent().getExtras();
        String time = bundle.getString("time");
        String content = bundle.getString("content");
        String desc = String.format("收到消息:\n请求时间:%s\n请求内容:%s", time, content);
        tv_request.setText(desc);

//        返回数据
        findViewById(R.id.btn_response).setOnClickListener(this);
    }

    @Override
    public void onClick(View v) {
        Intent intent = new Intent();
//       创建一个新的包裹
        Bundle bundle = new Bundle();
        bundle.putString("time", Utils.getNowTime());
        bundle.putString("content", tv_response.getText().toString());
//        包裹放入意图对象
        intent.putExtras(bundle);
//       回传数据 RESULT_OK表示处理成功
        setResult(Activity.RESULT_OK, intent);
//        结束活动
        finish();
    }
}
```

### **利用元数据传递配置信息** 

###### 元数据是一种描述其他数据的数据，它相当于描述固定活动的参数信息。 

###### 在activity节点内部添加meta-data标签，通过属性name指定元数据的名称，通过属性value指定元数据的值。

#### **在代码中获取元数据**

###### 在Java代码中，获取元数据信息的步骤分为下列三步： 

-  调用getPackageManager方法获得当前应用的包管理器； 

-  调用包管理器的getActivityInfo方法获得当前活动的信息对象； 

-  活动信息对象的metaData是Bundle包裹类型，调用包裹对象的getString即可获得指定名称的参数值； 

new > activity > MetaActivity

```xml
<activity
          android:name=".MetaActivity"
          android:exported="true"
          android:launchMode="standard">
    <intent-filter>
        <action android:name="android.intent.action.MAIN" />

        <category android:name="android.intent.category.LAUNCHER" />
    </intent-filter>
    
    <meta-data android:name="email" android:value="邮件页面"/>
</activity>
```

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">
    <TextView
        android:id="@+id/tv_msg"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:textSize="25sp"/>
</LinearLayout>
```

```java
public class MetaActivity extends AppCompatActivity {
    private TextView tv_msg;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_meta);
        tv_msg = findViewById(R.id.tv_msg);
        showMetaData();
    }
    // 显示配置的元数据
    private void showMetaData() {
        try {
            PackageManager pm = getPackageManager();
            // 获取应用包管理器
            // 从应用包管理器中获取当前的活动信息
            ActivityInfo info = pm.getActivityInfo(getComponentName(), PackageManager.GET_META_DATA);
            Bundle bundle = info.metaData;
            // 获取活动附加的元数据信息
            String value = bundle.getString("email");
            // 从包裹中取出名叫weather的字符 串
            tv_msg.setText("来自元数据信息：" + value);
            // 在文本视图上显示文字
        } catch (PackageManager.NameNotFoundException e) {
            e.printStackTrace();
        }
    }
}
```

### **给应用页面注册快捷方式**

###### 元数据不仅能传递简单的字符串参数，还能传送更复杂的资源数据，比如支付宝的快捷方式菜单。

![image-20220919232959983](C:\Users\15596\AppData\Roaming\Typora\typora-user-images\image-20220919232959983.png)

#### **利用元数据配置快捷菜单**

###### 元数据的meta-data标签除了前面说到的name属性和value属性，还拥有resource属性，该属性可指定一个XML文件，表示元数据想要的复杂信息保存于XML数据之中。 

###### 利用元数据配置快捷菜单的步骤如下所示： 

-  在res/values/strings.xml添加各个菜单项名称的字符串配置 

- 创建res/xml/shortcuts.xml，在该文件中填入各组菜单项的快捷方式定义（每个菜单对应哪个活动页面）。 

- 给activity节点注册元数据的快捷菜单配置。 

```xml
res/values/strings.xml
<resources>
    <string name="app_name">study03</string>
    <string name="first_short">first</string>
    <string name="first_long">邮件页面</string>
    <string name="two_short">two</string>
    <string name="two_long">登录</string>
</resources>
```

new > res > xml > shortcuts.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<shortcuts xmlns:android="http://schemas.android.com/apk/res/android">
    <shortcut
        android:shortcutId="first"
        android:enabled="true"
        android:icon="@mipmap/ic_launcher"
        android:shortcutShortLabel="@string/first_short"
        android:shortcutLongLabel="@string/first_long">
        <intent
           android:action="android.intent.action.VIEW"
            android:targetPackage="com.zjgs.study03"
            android:targetClass="com.zjgs.study03.MetaActivity" />
        <categories android:name="android.shortcut.conversation"/>
    </shortcut>
    <shortcut
        android:shortcutId="two"
        android:enabled="true"
        android:icon="@mipmap/ic_launcher"
        android:shortcutShortLabel="@string/two_short"
        android:shortcutLongLabel="@string/two_long">
        <intent
            android:action="android.intent.action.VIEW"
            android:targetPackage="com.zjgs.study03"
            android:targetClass="com.zjgs.study03.LoginInputActivity" />
        <categories android:name="android.shortcut.conversation"/>
    </shortcut>
</shortcuts>
```

```xml
<activity
          android:name=".MetaActivity"
          android:exported="true"
          android:launchMode="standard">
    <intent-filter>
        <action android:name="android.intent.action.MAIN" />
        <category android:name="android.intent.category.LAUNCHER" />
    </intent-filter>
    <meta-data android:name="email" android:value="邮件页面"/>
    
    <meta-data android:name="android.app.shortcuts" android:resource="@xml/shortcuts"/>
</activity>
```

