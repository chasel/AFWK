# Activity和Process管理

本文介绍Android是如何管理程序Processes和Activities的，包括他们的生命周期、优先级和资源分配。

## 概述
Android's的Activity和Process管理系统主要包括以下职责：

* 启动、维护和结束程序Processes
* 管理Activity生命周期和状态转换
* 根据对用户的重要性对Processes进行优先级排序
* 通过Process优先级调整和终止来优化内存使用
* 处理前/后台应用状态的切换

![AMS概述](./assets/2.1-activity-and-process-management.png)

源代码：

* [services/core/java/com/android/server/am/ActivityManagerService.java](https://cs.android.com/android/platform/superproject/+/android-latest-release:frameworks/base/services/core/java/com/android/server/am/ActivityManagerService.java)
* [services/core/java/com/android/server/am/psc/OomAdjuster.java](https://cs.android.com/android/platform/superproject/+/android-latest-release:frameworks/base/services/core/java/com/android/server/am/psc/OomAdjuster.java)
* [services/core/java/com/android/server/am/ProcessList.java](https://cs.android.com/android/platform/superproject/+/android-latest-release:frameworks/base/services/core/java/com/android/server/am/ProcessList.java)
* [services/core/java/com/android/server/am/ActiveServices.java](https://cs.android.com/android/platform/superproject/+/android-latest-release:frameworks/base/services/core/java/com/android/server/am/ActiveServices.java)
* [services/core/java/com/android/server/wm/ActivityRecord.java](https://cs.android.com/android/platform/superproject/+/android-latest-release:frameworks/base/services/core/java/com/android/server/wm/ActivityRecord.java)

## Process生命周期和状态
Android Process会经历从创建到终止的生命周期。当某个组件（如Activity、Service、Broadcast Receiver或Content Provider）需要运行时，系统会启动该进程；而当系统需要回收资源时，该进程可能会被终止。

### Process状态
Process有各种各样的状态，它们反映了对用户的重要性：

![process states](./assets/2.1-process-states.png)

源代码：

* [core/java/android/app/ActivityManager.java](https://cs.android.com/android/platform/superproject/+/android-latest-release:frameworks/base/core/java/android/app/ActivityManager.java)
* [services/core/java/com/android/server/am/ProcessList.java](https://cs.android.com/android/platform/superproject/+/android-latest-release:frameworks/base/services/core/java/com/android/server/am/ProcessList.java)