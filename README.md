# Geofences

The "Geofences" sample app that introduces you to the geofence features of the ContextHub iOS SDK and developer portal.

## Purpose
This sample application will show you how to use Geofences to trigger events using the ContextHub iOS framework.

## ContextHub

In this sample application, we use ContextHub to trigger events on the server by setting up geofences on a map. ContextHub takes care of setting up and monitoring geofences to generate events that the server can listen and respond to and then save those events in a vault to be accessed later.

## Getting Started

1. Get started by either forking or cloning the Geofence repo. Visit [GitHub Help](https://help.github.com/articles/fork-a-repo) if you need help.
1. Go to [ContextHub](http://app.contexthub.com) and create a new Geofence application.
1. Find the app id associated with the application you just created. Its format looks something like this: `13e7e6b4-9f33-4e97-b11c-79ed1470fc1d`.
1. Open up your Xcode project and put the app id into the `[ContextHub registerWithAppId:]` method call.
1. Build and run the project in the simulator.

## Xcode Console
1. This sample demo will log events into the debug console to get an idea of the JSON structure posted to ContextHub. Use shortcut `Shift-⌘-Y` if your console is not already visible.
1. You should see the message "CCH: Device has successfully registered your application ID with ContextHub".
1. Within the application delegate are 4 methods defined by the `CCHSensorPipelineDataSource` and `CCHSensorPipelineDelegate` which allow you hook into the pipeline of events generated by the device sensors. You can get notified when an event will post and has been posted, as well as control if an event should be posted and add extra payload data to an event. These 4 methods allow a lot of flexibility in controlling what events get sent to the server.
1. Check out the ContextHub [documentation](http://docs.contexthub.com) for more information about the event service.
    
## Create a Geofence
1. Back in the simulator you'll see a map view that is centered at the current location.  Lets set your location to one of the major cities XCode provides (click on the compass arrow in the debug controls area of XCode in the bottom). Zoom in as closely as possible to street level where the location circle appears on the map.
1. Tap the "+" located at the top right of the application to create a new geofence.  Give it a name and tap okay.  This will create a geofence on the ContextHub server.

## ContextHub.com

1. The real power of ContextHub comes from collecting and reacting to events posted from devices onto the server. Go to the [developer portal](https://app.contexthub.com) and click on your  Geofence app to access its data.
1. Click on the "Geofences" tab.  You should see the geofence that you just created in the simulator.  From here you can create, update, and delete geofences.
1. Next click on the Contexts link which will take you to the "Contexts" page. Contexts let you change how the server will respond to events triggered by devices. Let's go ahead and create a new context.

## Creating a New Context

1. In the "Contexts" tab, click the "New Context" button to start making a new rule.
1. Enter a name for this context which will be easy for you to remember. For now, name it "Geofence In".
1. Select the `"geofence_in"` event type. Now any event of type `"geofence_in"` will trigger this rule. You can have multiple rules with the same event type, which is why the name of events should be descriptive of the rule.
1. The Context Rule text box is where you can write a rule telling ContextHub what actions to take in response to an event triggered with the specific event type. This code is Javascript, and you have access to some context objects: event, push, vault, http, and console. For now, leave the Code box blank and then click save.

## Build and Run
1. Back in XCode, stop and restart the simulator.
1. Simulate a location change in the device to one of the major cities XCode provides. 
1. Change the simulated location in XCode back to where you previously were and you should see a `"geofence_in"` event triggered and printed to the console. If you are having issues with events not being generated, see the note at the bottom for help.
1. In the ContextHub [developer portal](http://app.contexthub.com), you should be able to see events generated by your device appear under "Latest events", no code needed!


## Use the vault object in the context rule

Now that we've gotten the app to trigger events that both appear on the device and the server, it's time to create a rule that does something with that event.

1. Go back to "Contexts" and click on the `Geofence In` context.
1. This page is where you previously set the rule related to `"geofence_in"`. Let's save the event data to the vault using Javascript.
1. Like stated previously, context rules have access to objects: events, push, vault, http, and console. Events contain data about the event which triggered the rule. The vault object lets you persist data on the server for retrieval later. Take a look at the vault object by clicking on it and seeing what methods it has access to.
1. In order to create an object in the vault, type `vault.create(JSON.stringify(event), "sample")` into the Code box, then click save.  This will save the event in the vault and apply the "sample" tag to it.
1. Restart the simulator again, simulate a geofence_in event, and then go back to the developer portal to see if the event appeared in "Latest Events" under "Contexts".
1. Now click on "Vault" at the top to see if a "sample" tag with an item with event data was created. Each time the `"geofence_in"` event is received by ContextHub, it will create a new item in the vault for you to access based on the rule you just created.
1. That's all it takes to create a rule and put data into the vault!


## Trouble with the simulator
- If you hare having trouble with your location not appearing on the simulator, make sure you have the simulator set to use a custom location. Go to Debug-> Location, and set it to "Custom" to allow XCode to trigger changes manually.
- You can use the "Freeway" option to trigger "location_changed" events in your app as well.


