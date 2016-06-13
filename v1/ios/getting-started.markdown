---
layout: page
title:  "Getting Started"
tags: mobile, ios
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

## Step 1: Set up Apple Push Notification service (APNs) for your app

The MokiManage platform uses Apple Push Notification Service (APNs) to communicate with your application. In order to send messages to devices on your behalf, you will need to provide the appropriate certificates that you get from Apple.

For more detail on setting up your app for APNs, click the APNs Setup Guide below.

[APNs Setup Guide](/v1/ios/apns-setup-guide)

## Step 2: Add the MokiManage SDK to your project

Add MokiManage to your application using cocoapods.

	pod 'MokiManageSDK', :git => 'https://github.com/MokiMobility/MokiManageSDK.git', :tag => '1.2.11'

If you want to use CocoaPods, but do not already have an existing Podfile, see the [Cocoapods Getting Started](/v1/ios/getting-started-cocoapods) guide.

<!-- Note: We strongly encourage using CocoaPods. It will ease your configuration, help you get updates to our SDK, and help ensure that you have all the required dependencies. If you do choose to manually add the SDK to your project there is some more work to do.-->

## Step 3: Set up your Info.plist

### Add the App Id
Add a String entry to your target's `Info.plist` named `appId` and enter your App Id. This refers to one of the values from Step 0.

![App Id Example](/assets/appID_plist.png)

> **Note:** Replace any spaces in your App ID with `%20`. For example `SDK Test` becomes `SDK%20Test` 

### Add the cert types
Add a dictionary entry to your target's `Info.plist` named `certType` with 3 boolean items. The items are: `store`, `enterprise`, and `sandbox`.

![App Id Example](/assets/info.plist_442x82.png)

This entry tells the SDK which APNs cert the platform should use to communicate with the app. These entries map to the same cert entries you uploaded in MokiManage when you set up your APNs certs in Step 1. During development, mark `YES` for sandbox, and `NO` for the other entries.


## Step 4: Set up your app delegate

Update your app delegate to work with MokiManage and APNs.

### Import the header

Add the MokiManage.h header to your app delegate header file:

	import "MokiManage.h"

<!--Add the MokiManage protocol to your delegate:

	@interface AppDelegate : UIResponder <UIApplicationDelegate, MokiManageDelegate>
-->

### Define your App Key and Tenant Id 

In your AppDelegate.m file, add a declaration for your app key and tenant ID:

	#define APP_KEY @"whatever-your-app-key-is-useitherenow"

	#define TENANT_ID @"whatever-your-tenaant-id-is-useitnow"

### Initialize the SDK

Initiate the session with the MokiManage SDK from your app delegate’s `didFinishLaunchingWithOptions:` method, calling the `initializeWithApiKey:` method.

	NSError *error;

	[[MokiManage sharedManager] initializeWithApiKey:APP_KEY
	launchingOptions:launchOptions
	enableASM:NO
	enableAEM:YES
	enableComplianceChecking:NO
	asmSettingsFileName:nil
	error:&error];

	[[MokiManage sharedManager] setDelegate:self];

### Setup APNs delegate methods

Add the delegate methods for APNs. From the `didRegisterForRemoteNotificationsWithDeviceToken:` method, call MokiManage to pass on the device token. Now MokiManage has the device's APNs token and will use that to send APNs messages.

> Note: The iOS Simulator does not correctly register for an APNs token, which is required in order to register a device with MokiManage. In order to test the entire device registration lifecycle, you need to use a physical device.

Add the appropriate logic to the `didFailToRegisterForRemoteNotificationsWithError:` and `didReceiveRemoteNotification:` methods.

	- (void)application:(UIApplication*)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData*)deviceToken {
		[[MokiManage sharedManager] setApnsToken:deviceToken];
	}

	- (void)application:(UIApplication*)application didFailToRegisterForRemoteNotificationsWithError:(NSError*)error {
		if(error) {
			// Add error processing here.
			NSLog(@"error registering for push notifications %@",error);
		}
	}

	- (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo {
		[[MokiManage sharedManager] didReceiveRemoteNotification:userInfo];
	}

## Step 5: Register with MokiManage

The next step is to add the code that enrolls the device with MokiManage. This is done by adding a call to the silentlyRegisterDevice method. This can be placed at any point in your app's workflow.In this example, it is added inside of the delegate method didRegisterForRemoteNotificationsWithDeviceToken:

	- (void)application:(UIApplication*)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData*)deviceToken {
	   [[MokiManage sharedManager] setApnsToken:deviceToken];
	   [[MokiManage sharedManager] silentlyRegisterDevice:TENANT_ID];
	}

## Step 6 (Optional): Set up Notificaiton Observers

The follow notifications can be observed to get feedback when registration completes or fails to do so.

* `MMApplicationDidRegisterNotification`
* `MMApplicationDidUnRegisterNotification`
* `MMApplicationDidRegisterToNewTenantNotification`
* `MMApplicationDidFailToRegisterNotification`
* `MMApplicationDidFailToUnRegisterNotification`
* `MMApplicationDidFailToRegisterToNewTenantNotification`

There are many ways to observer notifications. This is one example:

	NSNotificationCenter *center = [NSNotificationCenter defaultCenter];
	[center addObserverForName:MMApplicationDidRegisterNotification object:nil queue:nil usingBlock:^(NSNotification *notification) {
    	NSLog(@"Application registered with Moki successfully.");
    }];

## Step 7: Check the MokiManage Console

You can now verify that your integration is working.

1. Load the app on your device
2. Sign in to mokimanage.com
3. Click the `Change app` drop-down in the top left corner

	> Note: If you do not see your app on the list, contact MokiManage support at 888-997-5505 or support@mokimobility.com.

4. Select your app and you will see a dashboard for your app
5. Click on the `Devices` at the top of the page
5. Select your device

	On the right you should see a tabbed panel showing information about the device you selected. The `Performance` tab shows a graphical representation of the logs the AEM module collects. If you select the `Support` tab, you’ll see information about your device, such as battery status, network information, and so on.

All of the device actions represented in the `Actions` drop-down menu are available without any additional configuration.
