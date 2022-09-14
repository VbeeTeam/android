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

示例，Activity3 设置为singleInstance，Activity1 和 Activity2 默认（standard），下图程序流程中，黄色的代表 Background 的Task，蓝色的代表 Foreground 的Task。返回时会先把 Foreground 的Task 中的 Activity 弹出，直到 Task 销毁，然后才将 Background的 Task 唤到前台，所以最后将Activity3 销毁之后，会直接退出应用。

#### **动态设置启动模式**

在上述所有情况，都是我们在Manifest中通过 launchMode 属性设置的，这个被称为静态设置，动态设置是通过 Java 代码设置的。

通过 Intent 动态设置 Activity启动模式

```java
intent.setFlags();
```

如果同时有动态和静态设置，那么动态的优先级更高。接下来我们来看一下如何动态的设置 Activity 启动模式。