---
layout: page
title:  "Events Programming Guide"
tags: mobile, android
--- 

Events will be sent to the server immediately and can be found on the events tab for a given device. Moki reports a variety of events automatically. You can send your own events in addition to these using the following sdk calls:

|	Parameter	| Description		|
|:--------------|:--------------|:--------------|
| `eventId`	 |	The identifier of the event type.	This identifier should be a machine readable name without spaces. Limited to 24 characters.|
| `severity` 	 | This value should be an integer in the range of 1 to 5.	|
| `description` | A human readable description of this particular event type. Limited to 255 characters. |
| `data`	 | An object containing a set of data related to the given event.	|

Use this method when you do not have any unique event data to add to the event

    sendEvent(String eventId, int severity, String description);

Use this method when you have only 1 `<key,value>` pair to add to the event

    sendEvent(String eventId, int severity, String description, String name, String value); 

Use this method when you have multiple `<key,value>` pairs to add to an event

    sendEvent(String eventId, int severity, String description, HashMap<String, Serializable> data);

### Example Event

Any data that gets set in the "setData" item is deserialized by calling "toString". to string is called on the value

	import java.util.HashMap;
	import com.moki.manage.api.MokiManage;
	
	// The type of this event as defined by the application. This value cannot be null or empty string.
	String typeId = "001";
	
	// The level of severity of this event on a scale of 1-5, 5 being the most severe.  
	int severity = 1;
	
	// Set a human readable description for the event
	String description = "A human readable description";
	
	// Add unique data points to the event that provide additional details about the event
	HashMap<java.lang.String,java.io.Serializable> data = new HashMap<>();
	data.put("key1","value1");
	data.put("key2","value2");
	
	MokiManage.sharedInstance().sendEvent(typeId, severity, description, data);



