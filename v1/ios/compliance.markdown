---
layout: page
title:  "Compliance Programming Guide"
tags: mobile, ios
---

The Compliance module of the MokiManage SDK provides a way to quickly have your app report on a number of compliance checks that cover both the app and the device the app is running on. By reporting items like if the device is rooted/jailbroken or if it is running on an old version of the OS, you can make functionality decisions in your app based on the current compliance status.

A Compliance Report is a collection of compliance checks performed on the device to give a snapshot of security. The structure of this report in iOS is comprised of a ComplianceReport class with information about the overall check and an array of ComplianceCheck class objects. Documentation for these classes can be found in the [SDK Apple Docs](/ios/appledocs/).

There are 3 ways to use compliance reports in your application. The first is to initialize compliance checking when you initialize the `MokiManage sharedManager` in the `AppDelegate.m` file.

	[[MokiManage sharedManager] initializeWithApiKey:appKey
	                                launchingOptions:[appDelegate launchingOptions]
	                                       enableASM:YES
	                                       enableAEM:YES
	                        enableComplianceChecking:YES
	                             asmSettingsFileName:nil
	                                           error:&error];
	
When this option is enabled a Compliance Report will be run on every heartbeat. By default this happens every hour. If the heartbeat timer is customized, the compliance report frequency will change accordingly. This method provides a simple, continually updated view to the compliance status of the app and device on the mokimanage.com console or, optionally, within the app itself.

The second method is to look at last run compliance report as part of the compliance singleton. When a check is triggered manually on the singleton or by the automatic heartbeat, the results of the check are available by asking for the singleton and inspecting its properties. You can implement application logic around whether the device is in compliance or not.

	ComplianceReport *report = [[MokiManage sharedManager] complianceReport];
	
The third method is to run a compliance report manually. Triggering a check will run a compliance check locally and then push those results to mokimanage.com. After this is complete the you will be able to implement application logic around whether the device is in compliance or not as was mentioned before. Triggering a check manually will not effect the scheduled run time on a heartbeat, if it is turned on.

	[[[MokiManage sharedManager] complianceReport] run];
