---
layout: page
title:  "Follow Me Support"
tags: mobile, android
---

Follow Me Support enables support teams to more effectively troubleshoot problems by giving  them the ability to follow the screens and taps that a user makes on a mobile device. When  enabled, Follow Me Support captures a screen shot every time the user taps the screen, or  at a regular interval if the user isn’t touching the screen. These screenshots are uploaded to the MokiManage servers along with the touch coordinates and other metadata. The support  representative will be able to watch in near real-time what the device user is doing. Follow Me sessions are automatically saved and can be reviewed at any time.

Follow Me Support is activated by a request from the support representative on MokiManage.com. Once the request is sent, the device user will receive a request to begin a Follow Me session that he or she must accept in order to activate the session.

By implementing the MokiManage SDK in your app, Follow Me Support is available. No additional configuration is required.

When a Follow Me session is started, the MokiManage SDK will broadcast an intent with the action `MokiManage.FOLLOW_ME_INTENT` and with the category equal to your app’s package name.

Inside your BroadcastReceiver, you must call `MokiManage.sharedInstance().openFollowMeDialog(Activity activity,Intent intent)`. This will open a dialogue for the end user of your app to accept or decline the Follow Me session.

In your Activities, override the `dispatchTouchEvent` function as follows:

	@Override
	public boolean dispatchTouchEvent(MotionEvent ev) {
		FollowMeGestureDetector.sharedInstance().handleTouch(ev);
		return super.dispatchTouchEvent(ev);
	}

This should catch all of the gestures in that activity.

If you want to track touches yourself, call the `reportFollowMeAction` function passing in the current activity and `Point[]` of the touches. Also anytime your activity changes, you must call `reportNewFollowMeActivity`, passing in the new activity, otherwise the images the support representative sees will not reflect what the user is seeing.

You can call `endFollowMeSession` to end the session yourself, but the SDK will clean up and  eventually time out, and a notification will be broadcast that allows the user to end the session by dismissing the notification.

Note : Messaging service is required for Follow Me Support. Make sure that GCM is configured.

> Be aware that sensitive information could be visible in Follow Me sessions, and you may be liable for damages under your privacy policies.

## Initializing a Follow Me Support Session

1. Log on to MokiManage.com
1. From the Change App drop-down list in the top-left corner, select your app
1. Select the device that the user is using.
1. In the right pane, click the Support tab
1. Click Follow Me Support Sessions.
1. Click Request New Session
1. Once you initialize a Follow Me Support session, the device user receives a request to begin the Follow Me session. If accepted, the session activates.

To end the session, click End Session.

## Reviewing a Saved Follow Me Support Session

1. Log on to MokiManage.com
1. From the Change App drop-down list in the top-left corner, select your app.
1. Select the device that the user is using.
1. In the right pane, click the Support tab.
1. Click Follow Me Support Sessions.
1. From the Sessions drop-down list, select the session you want to review.
1. Click View.