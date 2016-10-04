App Component
=============
- Activities
- Services
- Content providers

Intent
=======
- Component name; w/o becomes implicit
- Action (str generic action to perform)
  - Action constants
    ACTION_VIEW, ACTION_SEND
- Data
  The Uri that references data to be acted upon
  ACTION_EDIT
  setDataAndType()
- Extra
  Bundle

Pending Intent
===============
Grand permission to foreign app to use the contained Intent as if it were executed from the app itself

Intent object is handled by: Activity, Service or BroadcastReceiver 

Lifecycle
=========
Application
-------------
onCreate()
onLowMemory()
onTrimMemory()
onTerminate()
onConfigurationChanged()

Content Provider
----------------

Activity
----------
Running, Paused, Stopped, Killed

onCreate(), onResume(), onPause(), onStop()

onSaveInstanceState() -> onRestoreInstanceState()
Bundle() - Parcelable()

# How check app private files
$ C:\Users\david\AppData\Local\Android\sdk\platform-tools\adb shell
$ run-as com.gtt.android.androidsms
$ ls files

# Access terminal
adb shell
# as root
adb root
adb shell
# Remount files after enabling root access (to refresh permission)
adb remount