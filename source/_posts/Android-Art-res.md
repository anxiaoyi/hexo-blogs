---
title: Android Art res
date: 2016-03-27 16:01:48
tags: book 
---

![book](http://img3.douban.com/lpic/s28283341.jpg)

<!--more-->

## Activity的生命周期和启动模式

### 生命周期

正常生命周期：`onCreate->onStart->onResume->onPause->onStop->onDestroy`

资源相关的配置发生改变导致Activity被杀死并重新创建：`Activity->onSaveInstanceState->onDestroy`，重新创建: `Activity->onCreate->onRestoreInstanceState`。如果不想重新创建，那么需要配置`android:configChanges="orientation|screenSize"`，这个时候系统将不会调用`onSaveInstanceState`和`onRestoreInstanceState`，取而代之的是调用`onConfigurationChanged`方法。

资源内存不足导致低优先级被杀死。

### 启动模式

- standard
  每次启动一个Activity，将会创建一个新的实例。谁启动这个Activity，那么这个Activity就运行在它的那个Activity栈中。

- singleTop
  栈顶复用模式。如果已经位于栈顶，那么此Activity不会被创建，同时它的`onNewIntent`方法会被回调。
  如果没有位于栈顶，那么此Activity仍然会被创建。
  `Intent.FLAG_ACTIVITY_SINGLE_TOP`

- singleTask
  栈内复用模式。单实例模式。只要栈内存在，就不会创建，同时`onNewIntent`方法也会被回调。
  `Intent.FLAG_ACTIVITY_NEW_TASK`

- singleInstance
  单实例模式。加强的`singleTask`模式，具有此模式的Activity只能单独的位于一个任务栈中。

命令`adb shell dumpsys activity`查看出栈入栈信息。

`Intent.FLAG_ACTIVITY_EXCLUDE_FROM_RECENTS`: 此Activity不会出现在历史Activity列表中。
`Intent.FLAG_ACTIVITY_CLEAR_TOP`: 一般会和`singleTask`启动模式一起出现，同一个任务栈位于其之上的所有Activity都要出栈。

### IntentFilter的匹配机制

## IPC 机制

启动：

- `android:process=":remote"`

`:`含义代表指在当前的进程名前面附加上包名，这是一种简写方法，其次表示此进程属于当前应用的私有进程，其它应用的组件不可以和它跑在同一个进程中。

- `android:process="com.zk.fast.player.remove"`

全局进程，其他应用通过`ShareUID`方式可以和它跑在同一个进程中(ShareUID相同，签名也相同)。

查看进程信息: `adb shell ps | grep com.zk.player`

多线程模式造成的问题：

- 静态成员和单例模式完全失效
- 线程同步机制完全失效
- SharedPreferences可靠性下降
- Application多次创建

### 序列化

通过`Intent`和`Binder`传输数据的时候我们就需要使用`Parceable`或者`Serializable`。

- Serializable

`private static final long serialVersionUID = 1L`，用以辅助序列化。
当成员变量的数量，类型发生变化之后，这个时候无法正常反序列化。指定了之后，在很大程度上避免反序列化失败。静态成员变量属于类不属于对象，所以不会参与序列化过程，`transient`标记的成员变量不参与序列化过程。

- `Parceable`

`Serializable`开销大，序列化与反序列化涉及大量I/O

### Binder

### IPC方式

- Bundle
实现了`Parcelable`接口。

- 文件共享

- Messenger
底层AIDL。服务端进程<==>客户端进程

服务端进程
```java
public class Server extends Service {
    private final Messenger mMessenger = new Messenger(new Handler() {
	@Override
	public void handleMessage(Message msg){
	    //receive message from client
	    switch (msg.what) {
	    case 0:
		
		// reply to client
		Messenger client = msg.replyTo;
		Message replyMessage = Message.obtain(null, 0);
		Bundle bundle = new Bundle();
		bundle.putString("reply", "Reply message");
		replyMessage.setData(bundle);
		try {
		    client.send( replyMessage );
		} catch (RemoteException e) {}

		break;
	    }
	}
    });
    @Override
    public IBinder onBind(Intent intent) {
	return mMessenger.getBinder();
    }
}
```

客户端进程
```java
public class ClientActivity extends Activity {

    private Messenger mService;

    private Messenger mReplyMessenger = new Messenger(new Handler() {
	@Override
	public void handleMessage(Message msg) {
	    // receive message from server
	}
    });

    private ServiceConnection mConnection = new ServiceConnection() {
	@Override
	public void onServiceConnected(ComponentName className, IBinder service) {
	    mService = new Messenger(service);

	    // send hello-world
	    Message msg = Message.obtain(null, 0);
	    Bundle data = new Bundle();
	    data.putString("msg", "hello world");
	    msg.setData(data);
	    msg.replyTo = mReplyMessenger;
	    try {
		mService.send(msg);
	    } catch( RemoteException ignore ) {}

	}
	@Override
	public void onServiceDisconnected(ComponentName className) {}
    }

    @Override
    protected void onCreate(Bundle savedInstanceState) {
	super.onCreate(savedInstanceState);
	Intent intent = new Intent(this, Server.class);
	bindService(intent, mConnection, Content.BIND_AUTO_CREATE);
    }

    @Override
    protected void onDestroy() {
	unbindService(mConnection);
	super.onDestroy();
    }
}
```

- AIDL

- ContentProvider
底层实现`Binder`

- Socket
