---
layout: page
title:  "Custom Actions Programming Guide"
tags: mobile, android
---

Custom Actions allow developers to create their own action references that can be triggered on  the device. MokiManage provides a set of pre-defined actions such as taking screenshots, getting  logs, getting device location, sending messages, and checking compliance. Developers have  the flexibility add their own action references that are specific to the needs of their app and its  users. Custom actions will be included alongside the MokiManage pre-defined actions on the  MokiManage dashboard.

When a custom action is received, the MokiManage SDK will rebroadcast that action with the intent action `MokiManage.CUSTOM_ACTION_INTENT`. The message of the action will be stored on the intent as a string extra with the  key `MokiManage.CUSTOM_ACTION_MESSAGE`. For more information on BroadcastReceiver and  Intents, see the [Android documentation](http://developer.android.com/reference/android/content/BroadcastReceiver.html).

## Example

#### `AndroidManifest.xml`

    <receiver
        android:name=".receivers.CustomActionReceiver"
        android:enabled="true"
        android:exported="true" >
        <intent-filter>
            <action android:name="com.moki.customaction" />
        </intent-filter>
    </receiver>

#### `CustomActionReceiver.java`

    public class CustomActionReceiver extends BroadcastReceiver {
        @Override
        public void onReceive(Context context, Intent intent) {
            String customAction = intent.getStringExtra("customActionMessage");
            Log.i(CustomActionReceiver.class.getSimpleName(), "custom action: " + customAction);
        }
    }

## Adding Custom Actions to the Actions List on the Dashboard

1. Log on to app.moki.com
1. From the Account drop-down in the top-right corner, select Developer Portal
1. Click Create Actions - Step 2
1. Select your app
1. Enter your custom action key
1. Click Add

Your custom action will now appear in the Action drop-down list on the Devices page of the dashboard.

## Running a Custom Action from Devices

1. Log on to app.moki.com
1. Click the Devices tab
1. Select the device on which you want to run the custom action.
1. From the Actions drop-down menu, select the custom action that you want to run.

## Running a Custom Action from Device Groups

1. Log on to app.moki.com
1. Click the Device Groups tab
1. Click Create Action Sequence on the Device Group tile you wish to use
1. Click the Moki-enabled Apps tab
1. Click the app where you added the custom action
1. Drag your custom action into the command sequence area
