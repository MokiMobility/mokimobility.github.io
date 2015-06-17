---
layout: page
title:  "Custom Actions Programming Guide"
tags: mobile, ios
---

Custom Actions allow developers to create their own action references that can be triggered on the device. MokiManage provides a set of pre-defined actions such as taking screenshots, getting logs, getting device location, sending messages, and checking compliance. Developers have  the flexibility add their own action references that are specific to the needs of their app and its  users. Custom actions will be included alongside the MokiManage pre-defined actions on the  MokiManage dashboard.

When a custom action is received, the `MMApplicationDidRecieveCustomActionNotification` is called. This notification conforms  to Apple NSNotifications that are broadcasted through the NS Notification Center. For more  information on this topic, see the [Apple documentation](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/Foundation/Classes/NSNotificationCenter_Class/).

The APNS notification received from MokiManage will be included in the userInfo of the notification. If you app has multiple custom actions defined, extract the action reference from  userInfo using [notification.userInfo objectForKey:@"command"].

Note: Messaging service is required for custom actions. Make sure that APNS is configured.

## Adding Custom Actions to the Actions List on the dashboard

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

> Note: This will call `MMApplicationDidRecieveCustomActionNotification` on the device.

## Examples

The following example demonstrates how to respond to a custom action if it were named "Show Current Time" in the moki dashboard.

	//
	//  AppDelegate.m
	//
	@implementation AppDelegate

	- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
		//............
		NSNotificationCenter *center = [NSNotificationCenter defaultCenter];
		[center addObserverForName:MMApplicationDidRecieveCustomActionNotification
		                    object:nil
		                     queue:nil
		                usingBlock:^(NSNotification *note) {

		    if ([note.userInfo objectForKey:@"Show Current Time"]) {
		        [self showCurrentTime];
		    }
		}];
		return YES;
	}
      
	- (void)showCurrentTime {
		NSDate *date = [NSDate dateWithTimeIntervalSinceNow:0];
		[[[UIAlertView alloc] initWithTitle:@"Current Time"
		                                message:[NSString stringWithFormat:@"Time: %@", date]
		                               delegate:nil
		                      cancelButtonTitle:@"Cool"
		                      otherButtonTitles: nil] show];
	}
      
