---
layout: page
title:  "Network Check Programming Guide"
tags: mobile, ios
---

The Network Check component of the MokiManage SDK adds network connectivity and  performance diagnostics to an app. Network Check is an effective diagnostic tool when users  of an app are experiencing connectivity problems. Using it can help support and development  personnel understand if there is a connectivity issue, and if so, which component within the network stack is not working.

Every time Network Check runs, it performs the following actions:

* Pings the default gateway
* Pings an outside host (designated as google.com)
* Pings MokiMobility
* Tests DNS connectivity and latency
* Validates that ports 80, 443, 5228, 5229, and 5230 are open.
* Issues a GET request to the developer-defined URLs, and indicates whether the specified text is found in the response.

Network Check can run in either Basic Check mode or Advanced Check mode:

* Basic Check : Runs a single Network Check at every heartbeat.
* Advanced Check : Does duration testing, and includes average latency, max latency, and missing packets. Advanced check does not run on the heartbeat—it must be triggered to be run. This test is ideal for support personnel when they are troubleshooting.

The results of every Network Check are uploaded to the MokiManage server. If the app is unable to contact the server, the results of the Network Check are stored on the device until it is able to  reach the server, at which point the stored Network Check results are uploaded.

##Adding URLs to Network Check

* Import the MMNetworkReport.h class into the source file you are coding in.
* In the addURL method, add the URLs and text strings that you want the Network Check to test. Note that the URL must be in its fully-qualified form.

The following examples show several possible scenarios:
	
	MMNetworkReport* networkReport = [MMNetworkReport new];
	
	[networkReport addURL:@"http://yahoo.com" checkForString:@"Example A" error:error];
	[networkReport addURL:@"http://mokimobility.com" checkForString:@"Example B" error:error];
	[networkReport addURL:@"http://YourServer.com:107" checkForString:nil error:error];
	
	[networkReport runBasicWithCompletionBlock:^(BOOL succeeded){
		
		// Get all report data in NSDictionary format
		NSDictionary* dictionaryReport = [networkReport encode];
		
		// Get individual checks and data
		NSArray* portList = [networkReport networkChecksForCheckType:MMNetworkCheckTypePortScans];
		MMNetworkCheck* networkCheck = [portList objectAtIndex:0];
		NSString* result = networkCheck.result;
	}];