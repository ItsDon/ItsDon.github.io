---
title: WindowManager&BadTokenException:Unable to add window
author: Don
date: 2018-4-23 14:24:23
tags:
    - Exception
    - Dialog
categories:
    - Tech
---

#### 详细的异常log
![BadTokenException](/images/BTE.jpeg)

#### 发生场景
   在异步任务执行完成后，调用了show dialog.

#### 原因分析
当异步任务完成后，调用该任务的activity可能处于被销毁或已销毁的阶段，即 `new Dialog(context)`所需的contex已不复存在。

#### 解决办法
在调用dialog的时候判断activity是否isFinishing()
    ```java
    if(!isFinishing()){
        showDialog();
    }

    ```

 <!-- more -->

#### 问题详细分析
网上找到了一篇Blog,对这种情况进行了简洁明了的说明，内容如下

Android – Displaying Dialogs From Background Threads

Having threads to do some heavy lifting and long processing in the background is pretty standard stuff. Very often you would want to notify or prompt the user after the background task has finished by displaying a Dialog.

The displaying of the Dialog has to happen on the UI thread, so you would do that either in the Handler object for the thread or in the onPostExecute method of an AsyncTask (which is a thread as well, just an easier way of implementing it). That is a textbook way of doing this and you would think that pretty much nothing wrong could go with this.

Surprisingly I found out that something CAN actually go wrong with this. After Google updated the Android Market and started giving crash reports to the developers I received the following exception:
```java
android.view.WindowManager$BadTokenException: Unable to add window — token android.os.BinderProxy@447a6748 is not valid; is your activity running?
at android.view.ViewRoot.setView(ViewRoot.java:468)
at android.view.WindowManagerImpl.addView(WindowManagerImpl.java:177)
at android.view.WindowManagerImpl.addView(WindowManagerImpl.java:91)
at android.view.Window$LocalWindowManager.addView(Window.java:424)
at android.app.Dialog.show(Dialog.java:239)
at android.app.Activity.showDialog(Activity.java:2488)
…
at android.os.Handler.dispatchMessage(Handler.java:99)
…

```

I only got a couple of these exceptions from thousands of installs, so I knew that was not anything that happens regularly or that it was easy to replicate.

Looking at the stack trace above it gives us a pretty good idea why it failed. It started in the Handler object, which naturally was called by a background thread after it finished its processing. The Handler instance tried to show a Dialog and before it could show it, it tried to set the View for it and then it failed with:
```
android.view.WindowManager$BadTokenException: Unable to add window — token android.os.BinderProxy@447a6748 is not valid; is your activity running?

```


The 447a6748 number is just a memory address of an object that no longer exists.

Note- do not get hung up on the exact number. It would be different with every execution.

Now we know why the application crashed, the only thing left is to figure out what caused it?

We know that background threads execute independently of the main UI thread. That means that the user could be interacting with the application during the time that the thread is doing its work under the covers. Well, what happens if the user hits the “Back” button on the device while the background thread is running and what happens to the Dialog that this thread is supposed to show? Well, if the timing is right the application will most likely crash with the above described error.

In other words what happens is that the Activity will be going through its destruction when the background thread finishes its work and tries to show a Dialog.

In this case it is almost certain that this should have been handled by the Virtual Machine. It should have recognized the fact that the Activity is in the process of finishing and not even attempted to show the Dialog. This is an oversight of the Google developers and it will probably be fixed some time in the future, but in the meantime the burden is on us to take care of this.

The fix to this is pretty simple. Just test if the Activity is going through its finishing phase before displaying the Dialog:
```java
private Handler myHandler = new Handler() {
@Override
public void handleMessage(Message msg) {
    switch (msg.what) {
    case DISPLAY_DLG:
        if (!isFinishing()) {
        showDialog(MY_DIALOG);
        }
    break;
    }
}
};

```

[原文链接](http://dimitar.me/android-displaying-dialogs-from-background-threads/)
