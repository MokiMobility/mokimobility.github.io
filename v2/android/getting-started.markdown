---
layout: page
title:  "Getting Started"
tags: mobile, android
---

## Step 0: Have an active Moki account
Before you start, you should already have an account on [mokimanage.com](https://www.mokimanage.com/). If you do not have one, request a [demo](https://moki.com/demo/).

You will need your App Key, App Id, and Tenant ID. This can be found by doing the following:

1. Login to the dashboard at [mokimanage.com](https://www.mokimanage.com/)
2. Click on the account name in the upper right corner
3. Click on "Account Settings"
4. Click on the "Developer Tools" section, identifed by the toolbox icon
5. The Tenant ID is listed towards the top
6. The App Key and App Id can be found associated with each app

## Step 1: Install Agent

Agent 3.0+ is a pre-requisite for the sdk. It must be installed and enrolled before anything can be done within an app containing the sdk.

## Step 2: Setup `build.gradle`

1. Include this repository:

		maven { url 'https://raw.github.com/MokiMobility/MokiManageSDK-Android/master/' }

	It may look something like this

		repositories {
			maven { url 'https://raw.github.com/MokiMobility/MokiManageSDK-Android/master/' }
			mavenLocal()
			mavenCentral()
		}

2. Include this dependency:

		compile 'com.moki:manage-sdk:2.0.2'
	
	It may look something like this  
		
		dependencies {
			compile fileTree(dir: 'libs', include: ['*.jar'])
			compile 'com.moki:manage-sdk:2.0.2'
		}
 
## Step 3: Setup `AndroidManifest.xml`

Include these receivers:
	
    <receiver 
        android:name="com.moki.ipc.IPCResponseReceiver"
        android:enabled="true"
        android:exported="true">
        <intent-filter android:priority="0">
            <action android:name="com.moki.ipc.AGENT_IPCService" />
        </intent-filter>
    </receiver>

    <receiver
        android:name="com.moki.ipc.IPCMessageReceiver"
        android:enabled="true"
        android:exported="true" >
        <intent-filter android:priority="1000">
            <action android:name="com.moki.ipc.SDK_IPCService" />
        </intent-filter>
    </receiver>

## Step 4: Initialize

The best place to do this is in your extended Application class.

1. Access your app key and app id

	These can be found on [mokimanage.com](https://www.mokimanage.com/app#settings/devSettings) and correlate to your account.
	
        public static final String APP_KEY = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx";
        public static final String APP_ID = "xxxxxxxxxx";
 
2. Initialize the sdk with the following function call

        MokiManage.sharedInstance(APP_KEY, APP_ID, this);

	For ease it is best to extend the android.os.Application class and specify that class name in your manifest like so:  

        <application android:name=".ExampleApplication"

### Example Application Class

Below is an example of what is needed in the subclass of android.os.Application.

    public class ExampleApplication extends Application {
        public static final String APP_KEY = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx";
        public static final String APP_ID = "xxxxxxxxxx";		    
		
        @Override
        public void onCreate() {
            super.onCreate();
            MokiManage.sharedInstance(APP_KEY, APP_ID, this);
        }
    }