---
title: "android developer problems"
layout: post
date: 2015-11-29
tags:
- andorid
categories:
- android
---

# Android Developer Problems

## 默认RecyclerView Item就有点击效果

```xml
android:clickable="true"
android:focusable="true"
android:background="?attr/selectableItemBackground" 
```

如果想要自定义ripple effect:

```xml
<?xml version="1.0" encoding="utf-8"?>
<ripple xmlns:android="http://schemas.android.com/apk/res/android"
    android:color="@color/colorHighlight">

    <item
        android:id="@android:id/mask"
        android:drawable="@color/windowBackground" />
</ripple>
```

旧版本：

```xml
<?xml version="1.0" encoding="utf-8"?>
<selector xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:state_pressed="true">
        <shape android:shape="rectangle">
            <solid android:color="@color/colorHighlight" />
        </shape>
    </item>
    <item>
        <shape android:shape="rectangle">
            <solid android:color="@android:color/transparent" />
        </shape>
    </item>
</selector>
```

## `RecycleView`怎样像`ListView`那样有`dividers`？

[ItemDecoration](http://stackoverflow.com/questions/24618829/how-to-add-dividers-and-spaces-between-items-in-recyclerview)

## Android 两个构造器互相新建对象：

```java
public class A {

    private B b;
    
    public A() {
        b = new B();
    }
    
}

public class B {

    private A a;

    public B() {
        a = new A();
    }
    
}
```

还有就是操作数据库的两个`Cursor`，互相创建对方引用的对象。

```java
06-02 15:48:47.607 20155-20244/x.y.z W/art: Large object allocation failed: ashmem_create_region failed for 'large object space allocation': Too many open files
06-02 15:48:47.607 20155-20244/x.y.z W/art: Large object allocation failed: ashmem_create_region failed for 'large object space allocation': Too many open files
06-02 15:48:47.607 20155-20244/x.y.z I/art: Forcing collection of SoftReferences for 15KB allocation
06-02 15:48:47.625 20155-20244/x.y.z I/art: Alloc concurrent mark sweep GC freed 3(96B) AllocSpace objects, 0(0B) LOS objects, 40% free, 7MB/13MB, paused 3.517ms total 17.887ms
06-02 15:48:47.627 20155-20244/x.y.z W/art: Large object allocation failed: ashmem_create_region failed for 'large object space allocation': Too many open files
06-02 15:48:47.627 20155-20244/x.y.z E/art: Throwing OutOfMemoryError "Failed to allocate a 15856 byte allocation with 5575136 free bytes and 88MB until OOM" (recursive case)
```

```java
06-02 15:14:14.151 23732-23808/x.y.z E/SQLiteLog: (14) cannot open file at line 30046 of [9491ba7d73]
06-02 15:14:14.151 23732-23808/x.y.z E/SQLiteLog: (14) os_unix.c:30046: (24) open(/data/data/x.y.z/databases/ppkefu.db-journal) - 
06-02 15:14:14.151 23732-23808/x.y.z E/SQLiteLog: (14) cannot open file at line 30046 of [9491ba7d73]
06-02 15:14:14.151 23732-23808/x.y.z E/SQLiteLog: (14) os_unix.c:30046: (24) open(/data/data/x.y.z/databases/ppkefu.db-journal) - 
06-02 15:14:14.151 23732-23808/x.y.z E/SQLiteLog: (14) statement aborts at 22: [SELECT conversation_uuid, conversation_name, conversation_icon, conversation_type, conversation_unread, conversation_latest_message_uuid, conversation_updatetime, conversation_status F
06-02 15:14:14.152 23732-23808/x.y.z E/SQLiteQuery: exception: unable to open database file (code 14); query: SELECT conversation_uuid, conversation_name, conversation_icon, conversation_type, conversation_unread, conversation_latest_message_uuid, conversation_updatetime, conversation_status FROM conversations WHERE conversation_uuid = ?
06-02 15:14:14.163 23732-23808/x.y.z E/AndroidRuntime: FATAL EXCEPTION: AsyncTask #4
                                                                      Process: x.y.z, PID: 23732
                                                                      java.lang.RuntimeException: An error occured while executing doInBackground()
                                                                          at android.os.AsyncTask$3.done(AsyncTask.java:304)
                                                                          at java.util.concurrent.FutureTask.finishCompletion(FutureTask.java:355)
                                                                          at java.util.concurrent.FutureTask.setException(FutureTask.java:222)
                                                                          at java.util.concurrent.FutureTask.run(FutureTask.java:242)
                                                                          at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1112)
                                                                          at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:587)
                                                                          at java.lang.Thread.run(Thread.java:818)
                                                                       Caused by: android.database.sqlite.SQLiteCantOpenDatabaseException: unable to open database file (code 14)
                                                                          at android.database.sqlite.SQLiteConnection.nativeExecuteForCursorWindow(Native Method)
                                                                          at android.database.sqlite.SQLiteConnection.executeForCursorWindow(SQLiteConnection.java:845)
                                                                          at android.database.sqlite.SQLiteSession.executeForCursorWindow(SQLiteSession.java:836)
                                                                          at android.database.sqlite.SQLiteQuery.fillWindow(SQLiteQuery.java:62)
                                                                          at android.database.sqlite.SQLiteCursor.fillWindow(SQLiteCursor.java:144)
                                                                          at android.database.sqlite.SQLiteCursor.getCount(SQLiteCursor.java:133)
                                                                          at android.database.AbstractCursor.moveToPosition(AbstractCursor.java:197)
                                                                          at android.database.AbstractCursor.moveToFirst(AbstractCursor.java:237)
                                                                          at x.y.z.service.db.ConversationDB.getConversation(ConversationDB.java:82)
                                                                          at x.y.z.service.db.MessagesDB.fromCursor(MessagesDB.java:143)
                                                                          at x.y.z.service.db.MessagesDB.getMessage(MessagesDB.java:73)
                                                                          at x.y.z.service.db.ConversationDB.fromCursor(ConversationDB.java:136)
                                                                          at x.y.z.service.db.ConversationDB.getConversation(ConversationDB.java:83)
                                                                          at x.y.z.service.db.MessagesDB.fromCursor(MessagesDB.java:143)
                                                                          at x.y.z.service.db.MessagesDB.getMessage(MessagesDB.java:73)
                                                                          at x.y.z.service.db.ConversationDB.fromCursor(ConversationDB.java:136)
                                                                          at x.y.z.service.db.ConversationDB.getConversation(ConversationDB.java:83)
                                                                          at x.y.z.service.db.MessagesDB.fromCursor(MessagesDB.java:143)
                                                                          at x.y.z.service.db.MessagesDB.getMessage(MessagesDB.java:73)
                                                                          at x.y.z.service.db.ConversationDB.fromCursor(ConversationDB.java:136)
                                                                          at x.y.z.service.db.ConversationDB.getConversation(ConversationDB.java:83)
                                                                          at x.y.z.service.db.MessagesDB.fromCursor(MessagesDB.java:143)
                                                                          at x.y.z.service.db.MessagesDB.getMessage(MessagesDB.java:73)
                                                                          at x.y.z.service.db.ConversationDB.fromCursor(ConversationDB.java:136)
                                                                          at x.y.z.service.db.ConversationDB.getConversation(ConversationDB.java:83)
                                                                          at x.y.z.service.db.MessagesDB.fromCursor(MessagesDB.java:143)
                                                                          at x.y.z.service.db.MessagesDB.getMessage(MessagesDB.java:73)
                                                                          at x.y.z.service.db.ConversationDB.fromCursor(ConversationDB.java:136)
                                                                          at x.y.z.service.db.ConversationDB.getConversation(ConversationDB.java:83)
                                                                          at x.y.z.service.db.MessagesDB.fromCursor(MessagesDB.java:143)
                                                                          at x.y.z.service.db.MessagesDB.getMessage(MessagesDB.java:73)
                                                                          at x.y.z.service.db.ConversationDB.fromCursor(ConversationDB.java:136)
                                                                          at x.y.z.service.db.ConversationDB.getConversation(ConversationDB.java:83)
                                                                          at x.y.z.service.db.MessagesDB.fromCursor(MessagesDB.java:143)
                                                                          at x.y.z.service.db.MessagesDB.getMessage(MessagesDB.java:73)
                                                                          at x.y.z.service.db.ConversationDB.fromCursor(ConversationDB.java:136)
                                                                          at x.y.z.service.db.ConversationDB.getConversation(ConversationDB.java:83)
                                                                          at x.y.z.service.db.MessagesDB.fromCursor(MessagesDB.java:143)
                                                                          at x.y.z.service.db.MessagesDB.getMessage(MessagesDB.java:73)
                                                                          at x.y.z.service.db.ConversationDB.fromCursor(ConversationDB.java:136)
                                                                      	at com.ppmes
06-02 15:14:14.185 23732-23808/x.y.z D/Error: ERR: exClass=android.database.sqlite.SQLiteCantOpenDatabaseException
06-02 15:14:14.185 23732-23808/x.y.z D/Error: ERR: exMsg=unable to open database file (code 14)
06-02 15:14:14.185 23732-23808/x.y.z D/Error: ERR: file=SQLiteConnection.java
06-02 15:14:14.185 23732-23808/x.y.z D/Error: ERR: class=android.database.sqlite.SQLiteConnection
06-02 15:14:14.185 23732-23808/x.y.z D/Error: ERR: method=nativeExecuteForCursorWindow line=-2
06-02 15:14:14.186 23732-23808/x.y.z D/Error: ERR: stack=java.lang.RuntimeException: An error occured while executing doInBackground()
                                                                 at android.os.AsyncTask$3.done(AsyncTask.java:304)
                                                                 at java.util.concurrent.FutureTask.finishCompletion(FutureTask.java:355)
                                                                 at java.util.concurrent.FutureTask.setException(FutureTask.java:222)
                                                                 at java.util.concurrent.FutureTask.run(FutureTask.java:242)
                                                                 at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1112)
                                                                 at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:587)
                                                                 at java.lang.Thread.run(Thread.java:818)
                                                              Caused by: android.database.sqlite.SQLiteCantOpenDatabaseException: unable to open database file (code 14)
                                                                 at android.database.sqlite.SQLiteConnection.nativeExecuteForCursorWindow(Native Method)
                                                                 at android.database.sqlite.SQLiteConnection.executeForCursorWindow(SQLiteConnection.java:845)
                                                                 at android.database.sqlite.SQLiteSession.executeForCursorWindow(SQLiteSession.java:836)
                                                                 at android.database.sqlite.SQLiteQuery.fillWindow(SQLiteQuery.java:62)
                                                                 at android.database.sqlite.SQLiteCursor.fillWindow(SQLiteCursor.java:144)
                                                                 at android.database.sqlite.SQLiteCursor.getCount(SQLiteCursor.java:133)
                                                                 at android.database.AbstractCursor.moveToPosition(AbstractCursor.java:197)
                                                                 at android.database.AbstractCursor.moveToFirst(AbstractCursor.java:237)
                                                                 at x.y.z.service.db.ConversationDB.getConversation(ConversationDB.java:82)
                                                                 at x.y.z.service.db.MessagesDB.fromCursor(MessagesDB.java:143)
                                                                 at x.y.z.service.db.MessagesDB.getMessage(MessagesDB.java:73)
                                                                 at x.y.z.service.db.ConversationDB.fromCursor(ConversationDB.java:136)
                                                                 at x.y.z.service.db.ConversationDB.getConversation(ConversationDB.java:83)
                                                                 at x.y.z.service.db.MessagesDB.fromCursor(MessagesDB.java:143)
                                                                 at x.y.z.service.db.MessagesDB.getMessage(MessagesDB.java:73)
                                                                 at x.y.z.service.db.ConversationDB.fromCursor(ConversationDB.java:136)
                                                                 at x.y.z.service.db.ConversationDB.getConversation(ConversationDB.java:83)
                                                                 at x.y.z.service.db.MessagesDB.fromCursor(MessagesDB.java:143)
                                                                 at x.y.z.service.db.MessagesDB.getMessage(MessagesDB.java:73)
                                                                 at x.y.z.service.db.ConversationDB.fromCursor(ConversationDB.java:136)
                                                                 at x.y.z.service.db.ConversationDB.getConversation(ConversationDB.java:83)
                                                                 at x.y.z.service.db.MessagesDB.fromCursor(MessagesDB.java:143)
                                                                 at x.y.z.service.db.MessagesDB.getMessage(MessagesDB.java:73)
                                                                 at x.y.z.service.db.ConversationDB.fromCursor(ConversationDB.java:136)
                                                                 at x.y.z.service.db.ConversationDB.getConversation(ConversationDB.java:83)
                                                                 at x.y.z.service.db.MessagesDB.fromCursor(MessagesDB.java:143)
                                                                 at x.y.z.service.db.MessagesDB.getMessage(MessagesDB.java:73)
                                                                 at x.y.z.service.db.ConversationDB.fromCursor(ConversationDB.java:136)
                                                                 at x.y.z.service.db.ConversationDB.getConversation(ConversationDB.java:83)
                                                                 at x.y.z.service.db.MessagesDB.fromCursor(MessagesDB.java:143)
                                                                 at x.y.z.service.db.MessagesDB.getMessage(MessagesDB.java:73)
                                                                 at x.y.z.service.db.ConversationDB.fromCursor(ConversationDB.java:136)
                                                                 at x.y.z.service.db.ConversationDB.getConversation(ConversationDB.java:83)
                                                                 at x.y.z.service.db.MessagesDB.fromCursor(MessagesDB.java:143)
                                                                 at x.y.z.service.db.MessagesDB.getMessage(MessagesDB.java:73)
                                                                 at x.y.z.service.db.ConversationDB.fromCursor(ConversationDB.java:136)
                                                                 at x.y.z.service.db.ConversationDB.getConversation(ConversationDB.java:83)
                                                                 at x.y.z.service.db.MessagesDB.fromCursor(MessagesDB.java:143)
                                                                 at x.y.z.service.db.MessagesDB.getMessage(MessagesDB.java:73)
                                                                 at x.y.z.service.db.ConversationDB.fromCursor(ConversationDB.java:136)
                                                             	at x.y.z.service.db.ConversationDB.getConversation(ConversationDB.j
06-02 15:14:14.187 23732-23808/x.y.z D/Error: ERR: TOTAL BYTES WRITTEN: 334532
```

Nested Cursor Problem

Table A has something stored in Table B, Table B has something stored in Table A

Android ListView Drawable Problem

单个Small异步任务 --(转变)--> 一个Big同步任务 ---RunWith---> 放到另外一个线程单独Run

Task 要可以 Cancel

使用大量数据校验

ProgressDialog 调用 dismiss 使其消失，而不是 hide，否则这个 Activity finish之后，可能会引发 Activity Memory Leak Problem

遇到下面异常，应该考虑升级`Android Studio`和相关`Android Support Library`

VFY: replacing opcode 0x6f at 0x008f
06-08 12:36:19.944 27636-27636/x.y.z I/dalvikvm: DexOpt: illegal method access (call Landroid/support/v4/app/Fragment;.performPrepareOptionsMenu (Landroid/view/Menu;)Z from Lcom/ppmessage/ppkefu/ui/fragment/SettingsFragment;)
06-08 12:36:19.944 27636-27636/x.y.z I/dalvikvm: Could not find method android.support.v4.app.Fragment.performPrepareOptionsMenu, referenced from method x.y.z.ui.fragment.SettingsFragment.access$super
06-08 12:36:19.944 27636-27636/x.y.z W/dalvikvm: VFY: unable to resolve virtual method 4265: Landroid/support/v4/app/Fragment;.performPrepareOptionsMenu (Landroid/view/Menu;)Z
06-08 12:36:19.944 27636-27636/x.y.z D/dalvikvm: VFY: replacing opcode 0x6f at 0x0098
06-08 12:36:19.954 27636-27636/x.y.z I/dalvikvm: DexOpt: illegal method access (call Landroid/support/v4/app/Fragment;.performStop ()V from Lcom/ppmessage/ppkefu/ui/fragment/SettingsFragment;)
06-08 12:36:19.954 27636-27636/x.y.z I/dalvikvm: Could not find method android.support.v4.app.Fragment.performStop, referenced from method x.y.z.ui.fragment.SettingsFragment.access$super
06-08 12:36:19.954 27636-27636/x.y.z W/dalvikvm: VFY: unable to resolve virtual method 4270: Landroid/support/v4/app/Fragment;.performStop ()V
06-08 12:36:19.954 27636-27636/x.y.z D/dalvikvm: VFY: replacing opcode 0x6f at 0x00a2
06-08 12:36:19.954 27636-27636/x.y.z I/dalvikvm: DexOpt: illegal method access (call Landroid/support/v4/app/Fragment;.performCreateOptionsMenu (Landroid/view/Menu;Landroid/view/MenuInflater;)Z from Lcom/ppmessage/ppkefu/ui/fragment/SettingsFragment;)
06-08 12:36:19.954 27636-27636/x.y.z I/dalvikvm: Could not find method android.support.v4.app.Fragment.performCreateOptionsMenu, referenced from method x.y.z.ui.fragment.SettingsFragment.access$super
06-08 12:36:19.954 27636-27636/x.y.z W/dalvikvm: VFY: unable to resolve virtual method 4257: Landroid/support/v4/app/Fragment;.performCreateOptionsMenu (Landroid/view/Menu;Landroid/view/MenuInflater;)Z
06-08 12:36:19.954 27636-27636/x.y.z D/dalvikvm: VFY: replacing opcode 0x6f at 0x00af
06-08 12:36:19.954 27636-27636/x.y.z I/dalvikvm: DexOpt: illegal method access (call Landroid/support/v4/app/Fragment;.performOptionsItemSelected (Landroid/view/MenuItem;)Z from Lcom/ppmessage/ppkefu/ui/fragment/SettingsFragment;)
06-08 12:36:19.954 27636-27636/x.y.z I/dalvikvm: Could not find method android.support.v4.app.Fragment.performOptionsItemSelected, referenced from method x.y.z.ui.fragment.SettingsFragment.access$super
06-08 12:36:19.954 27636-27636/x.y.z W/dalvikvm: VFY: unable to resolve virtual method 4262: Landroid/support/v4/app/Fragment;.performOptionsItemSelected (Landroid/view/MenuItem;)Z
06-08 12:36:19.954 27636-27636/x.y.z D/dalvikvm: VFY: replacing opcode 0x6f at 0x014b
06-08 12:36:19.954 27636-27636/x.y.z I/dalvikvm: DexOpt: illegal method access (call Landroid/support/v4/app/Fragment;.performConfigurationChanged (Landroid/content/res/Configuration;)V from Lcom/ppmessage/ppkefu/ui/fragment/SettingsFragment;)
06-08 12:36:19.954 27636-27636/x.y.z I/dalvikvm: Could not find method android.support.v4.app.Fragment.performConfigurationChanged, referenced from method x.y.z.ui.fragment.SettingsFragment.access$super
06-08 12:36:19.954 27636-27636/x.y.z W/dalvikvm: VFY: unable to resolve virtual method 4254: Landroid/support/v4/app/Fragment;.performConfigurationChanged (Landroid/content/res/Configuration;)V
06-08 12:36:19.954 27636-27636/x.y.z D/dalvikvm: VFY: replacing opcode 0x6f at 0x01dc

## Mutiple types of ListView Reuse

The Wrong Solution:

```java
@Override
public View getView(int position, View convertView, ViewGroup parent) {
       // 1. find type
       int type = getItemViewType(position);

       View v = null;
       if (type == TYPE_1) {
          ViewHolderType1 type1Holder = null;
          if (convertView == null) {
             convertView = inflater(R.layout.type_1, parent, false);
             type1Holder = new ViewHolderType1();
             convertView.setTag(type1Holder);
          } else {
            type1Holder = (ViewHolderType1) convertView.getTag();
          }

          // Do something with type1Holder

       } else if (type == TYPE_2) {
         ViewHolderType2 type2Holder = null;
         if (convertView == null) {
            convertView = inflater(R.layout.type_2, parent, false);
            type2Holder = new ViewHolderType2();
            convertView.setTag(type2Holder);
         } else {
           type2Holder = (ViewHolderType2) convertView.getTag();
         }

         // Do something with type2Holder

       }

       return v;
}

```

Sometimes, it will crash with exception :

```java
java.lang.ClassCastException: ViewHolderType2 cannot be cast to ViewHolderType1
```

The Right Solution:

```java
if (convertView == null || convertView.getTag().getClass() != ViewHolderType1.class) {
}

if (convertView == null || convertView.getTag().getClass() != ViewHolderType2.class) {
}

...
```

References:

[StackOverflow-Reusing-Views](http://stackoverflow.com/questions/13297299/reusing-views-in-android-listview-with-2-different-layouts)

[SpareTimeDev](http://sparetimedev.blogspot.my/2012/10/recycling-of-views-with-heterogeneous.html)

[Implementing a Heterogenous ListView](https://guides.codepath.com/android/Implementing-a-Heterogenous-ListView)

## Android Global Exception Handler

[Android Global Exception Handler](http://blog.csdn.net/singwhatiwanna/article/details/17289479)

