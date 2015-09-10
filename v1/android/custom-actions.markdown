---
layout: page
title:  "Custom Actions Programming Guide"
tags: mobile, android
---

Custom Actions allow developers to create their own action references that can be triggered on  the device. MokiManage provides a set of pre-defined actions such as taking screenshots, getting  logs, getting device location, sending messages, and checking compliance. Developers have  the flexibility add their own action references that are specific to the needs of their app and its  users. Custom actions will be included alongside the MokiManage pre-defined actions on the  MokiManage dashboard.

When a custom action is received, the MokiManage SDK will rebroadcast that action with the intent action `MokiManage.CUSTOM_ACTION_INTENT` and with the category equal to the app’s  package name. The message of the action will be stored on the intent as a string extra with the  key `MokiManage.CUSTOM_ACTION_MESSAGE`. For more information on BroadcastReceiver and  Intents, see the [Android documentation](http://developer.android.com/reference/android/content/BroadcastReceiver.html).

The APNS notification received from MokiManage will be included in the userInfo of the notification. If you app has multiple custom actions defined, extract the action reference from  userInfo using [notification.userInfo objectForKey:@"command"].

Note: Messaging service is required for custom actions. Make sure that GCM is configured.

## Adding Custom Actions to the Actions List on the Dashboard

1. Log on to MokiManage.com
1. From the Change App drop-down list in the top-left corner, select your app
1. Click the Actions Tab
1. Click Custom Actions
1. In the Custom Actions Name field, give your new action a name
1. Click Add, then click Done

Your custom action will now appear in the Action drop-down list on the Devices page of the dashboard.

## Running a Custom Action from the dashboard

1. Log on to MokiManage.com
1. From the Change App drop-down list in the top-left corner, select your app
1. Click the Devices tab.
1. Select the device or devices on which you want to run the custom action.
1. From the  Actions  drop-down menu, select the custom action that you want to run.