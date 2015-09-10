---
layout: page
title:  "Compliance Programming Guide"
tags: mobile, android
---

The Compliance module of the MokiManage SDK provides a way to quickly have your app report on a number of compliance checks that cover both the app and the device the app is running on. By reporting items like if the device is rooted/jailbroken or if it is running on an old version of the OS, you can make functionality decisions in your app based on the current compliance status.

A Compliance Report is a collection of compliance checks performed on the device to give a snapshot of security. The structure of this report is comprised of a ComplianceReport class with information about the overall check and an array of ComplianceCheck class objects. Documentation for these classes can be found in the [SDK Java Docs](http://mokimobility.github.io/MokiManageSDK/javadoc/com/moki/manage/api/MokiManage.html).

There are 2 ways to use compliance reports in your application. The first is to initialize compliance checking when you initialize the `MokiManage sharedManager` in the CustomApplication.java file.

	public class CustomApplication extends Application implements Application.ActivityLifecycleCallbacks {
		private MokiManage mmanage;
		private int activeActivityCount = 0;
		private final boolean enableASM = false;
		private final boolean enableAEM = true;
		private final boolean enableCompliance = true;
		private final String appKey = "";
		private final String appID = "";
		private final static Application instance;
		@Override
		public void onCreate() {
			super.onCreate();
			Context context = this;
			mmanage = MokiManage.sharedInstance(appKey, appID, context, enableASM, enableAEM, enableCompliance);
			registerActivityLifecycleCallbacks(this);
			instance = this;
		}
	}
	
When this option is enabled a Compliance Report will be run on every heartbeat. By default this happens every hour. If the heartbeat timer is customized, the compliance report frequency will change accordingly. This method provides a simple, continually updated view to the compliance status of the app and device on the mokimanage.com console or, optionally, within the app itself.

The second method is to utilize the `IComplianceReportCallback` interface. This will allow you to manually get the latest compliance information and utilize it in your application logic.

	mokiManage.getComplianceReport(new IComplianceReportCallback() {
		@Override
		public void reportReceived(ComplianceReport report) {
			if (report != null) {
				//ui work here
			} else {
				//error out or retry here
			}
		}
	});
	
There is the rare possibility that the return value could be null. This can happen when a compliance report is requested before it is on the server. Just be aware to check for that case in your implementation.

