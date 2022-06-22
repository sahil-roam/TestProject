# Listeners

Now that the location tracking is set up, you can subscribe to locations
and events and use the data locally on your device or send it directly
to your own backend server.

To do that, you need to set the location and event listener to `true`
using the below method. By default the status will set `false` and needs
to be set `true` in order to stream the location and events updates to
the same device or other devices.

<div class="code panel pdl" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` dart
Roam.toggleListener(locations: true,events: true,callBack: ({user}) {
                          //Do something with user
                          print(user);
                        });
```

</div>

</div>

You can also get the current status of listeners with the below method.

<div class="code panel pdl" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` dart
Roam.getListenerStatus(callBack: ({user}) {
                      //Do something with user
                      print(user);
                    });
```

</div>

</div>

# Subscribe Location and Trip Status

Now that you have enabled the location listener, use the below method to
subscribe to your own or other user's location updates and events. 

<div class="code panel pdl" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` dart
//subscribe to own location updates
Roam.subscribeLocation();

//subscribe to other user's location updates
Roam.subscribeUserLocation("ROAM-USER-ID");

//subscribe to trip status
Roam.subscribeTripStatus("ROAM-TRIP-ID");
```
</div>

</div>

You should set the location and event listener to `false` if you do not
need to stream the user location.

# Toggle Event Flags

To listen to events on the server-side, you should enable events for the
user using the method
below.

<div class="code panel pdl" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` dart
Roam.toggleEvents(location: true,geofence: true,trips: true,movingGeofence: true,callBack: ({user}) {
                          //Do something with user
                          print(user);
                        });
```

</div>

</div>

# Trips V1

***NOTE***: Trips V1 below is supported only up to 0.0.X. Trips V2 support starts from 0.1.X versions.

Use the below code to create a trip directly from the SDK. Set
**Boolean** value `true` to create offline trip and `false` to create
online trip.

<div class="code panel pdl" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` dart
Roam.createTrip(isOffline: false,callBack: ({trip}) {
                          //Do something with trip
                          print(trip);
                        });
```
</div>

</div>

Use the below code to start the trip with the previously created trip
id.

<div class="code panel pdl" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` dart
Roam.startTrip(tripId: <TRIP-ID>);
```

</div>

</div>

Use the below code to pause the trip with the previously started trip
id.

<div class="code panel pdl" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` dart
Roam.pauseTrip(tripId: <TRIP-ID>);
```

</div>

</div>

To resume the trip.

<div class="code panel pdl" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` dart
Roam.resumeTrip(tripId: <TRIP-ID>);
```

</div>

</div>

To end the trip.

<div class="code panel pdl" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` dart
Roam.endTrip(tripId: <TRIP-ID>);
```

To get the trip summary.

<div class="code panel pdl" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` dart
Roam.getTripSummary(tripId: <TRIP-ID>);
```


#Trips V2

##Quick Trip

A `RoamTrip` object must be passed for quickTrip.
Set isLocal **Boolean** value `true` to create offline trip and `false` to create
online trip.
Stops can be added using the `stop` property of the `RoamTrip` object.
Tracking mode for the trip can be set using the `trackingMode` parameter.

<div class="code panel pdl" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` dart
RoamTrip quickTrip = RoamTrip(isLocal: false);
                        
RoamTripStops stop = RoamTripStops(radius, [longitude,latitude]);
quickTrip.stop.add(stop);

Roam.startTrip(({roamTripResponse}) {
                          print(roamTripResponse?.toJson());
                          //do something with roamTripResponse object
                        }, ({error}) {
                          print(error?.toJson());
                        },
                        roamTrip: quickTrip,
                        roamTrackingMode: RoamTrackingMode.ACTIVE);
```

</div>

</div>

Roam has three default tracking modes along with a custom version.

<div class="code panel pdl" style="border-width: 1px;">

<div class="codeContent panelContent pdl">


``` dart
RoamTrackingMode.ACTIVE

RoamTrackingMode.BALANCED

RoamTrackingMode.PASSIVE
```

Custom Tracking for android

``` dart
RoamTrackingMode.time(updateInterval, desiredAccuracy: DesiredAccuracy.HIGH);

RoamTrackingMode.distance(distanceFilter, stopDuration, desiredAccuracy: DesiredAccuracy.HIGH);
```

Custom Tracking for iOS

``` dart
RoamTrackingMode.customIOS(activityType, desiredAccuracyIOS, allowBackgroundLocationUpdates, pausesLocationUpdatesAutomatically, showsBackgroundLocationIndicator, accuracyFilter, distanceFilter, updateInterval);
```

</div>

</div>

##Create Planned Trip

Use the below code to create a trip using the `RoamTrip` class. Set
**Boolean** isLocal value `true` to create offline trip and `false` to create
online trip.
Stops can be added using the `stop` property of the `RoamTrip` object.

<div class="code panel pdl" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` dart
RoamTrip roamTrip = RoamTrip(isLocal: false);

RoamTripStops stop = RoamTripStops(radius, [longitude,latitude]);

roamTrip.stop.add(stop);

Roam.createTrip(roamTrip, ({roamTripResponse}) {
                          print(roamTripResponse?.toJson());
                          //do something with roamTripResponse object
                        }, ({error}) {
                          String errorString = jsonEncode(error?.toJson());
                          print(errorString);
                        });
```

</div>

</div>

##Start Planned Trip

To start a previously created trip, pass the `trip id` in `startTrip` method

<div class="code panel pdl" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` dart
Roam.startTrip(({roamTripResponse}) {
 	print(roamTripResponse?.toJson());
	//do something with roamTripResponse object
 }, ({error}) {
 	print(error?.toJson());
 },
 tripId: tripId);

```

</div>

</div>

##Update Trip

To update an existing trip, create a `RoamTrip` object with `isLocal` boolean value and `tripID`.

<div class="code panel pdl" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` dart
RoamTrip updateTrip = RoamTrip(tripId: tripId);
updateTrip.isLocal = false;
updateTrip.description = "changed description";
Roam.updateTrip(updateTrip, ({roamTripResponse}) {
 	print(roamTripResponse?.toJson());
 	//do something with roamTripResponse object
 }, ({error}) {
  	print(error?.toJson());
 });
```

</div>

</div>

##Pause Trip

To pause a running trip, pass the `trip id` to `pauseTrip()` method.

<div class="code panel pdl" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` dart
Roam.pauseTrip(tripId, ({roamTripResponse}) {
  	print(roamTripResponse?.toJson());
//do something with roamTripResponse object
}, ({error}) {
 print(error?.toJson());
 ```

</div>

</div>

##Resume Trip

To resume a paused trip, pass the `trip id` to `resumeTrip()` method.

<div class="code panel pdl" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` dart
Roam.resumeTrip(tripId, ({roamTripResponse}) {
  	print(roamTripResponse?.toJson());
	//do something with roamTripResponse object
}, ({error}) {
 	print(error?.toJson());
});
 ```

</div>

</div>

##End Trip

To end an existing trip, pass the `trip id` and ***bool*** value to stop tracking in `endTrip()` method.


<div class="code panel pdl" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` dart
Roam.endTrip(tripId, false, ({roamTripResponse}) {
  	print(roamTripResponse?.toJson());
	//do something with roamTripResponse object
 }, ({error}) {
 	print(error?.toJson());
});
 ```

</div>

</div>

##Delete Trip

To delete a trip, pass the `trip id` in `deleteTrip()` method.

<div class="code panel pdl" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` dart
Roam.deleteTrip(tripId, ({roamDeleteTripResponse}) {
  	print(roamDeleteTripResponse?.toJson());
	//do something with roamDeleteTripResponse object
 }, ({error}) {
 	print(error?.toJson());
});
 ```

</div>

</div>

##Sync Trip

To sync an offline trip, pass the `trip id` in the `syncTrip` method.

<div class="code panel pdl" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` dart
Roam.syncTrip(tripId, ({roamSyncTripResponse}) {
 	 print(roamSyncTripResponse?.toJson());
}, ({error}) {
 print(error?.toJson());
});
```

</div>

</div>

##Get Trip

To get details of a trip, pass the `trip id` in `getTrip()` method.

<div class="code panel pdl" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` dart
Roam.getTrip(tripId, ({roamTripResponse}) {
  	print(roamTripResponse?.toJson());
	//do something with roamTripResponse object
 }, ({error}) {
 	print(error?.toJson());
});
```

</div>

</div>

##Get Active Trips

To get active trips, pass ***bool*** value as `true` for offline trips and `false` for online trips.

<div class="code panel pdl" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` dart
Roam.getActiveTrips(false, ({roamActiveTripResponse}) {
 	print(roamActiveTripResponse?.toJson());
}, ({error}) {
 print(error?.toJson());
});
```

</div>

</div>

##Get Trip Summary

To get the trip summary with route coordinates, pass `trip id` in the `getTripSummary()` method.

<div class="code panel pdl" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` dart
Roam.getTripSummary(tripId, ({roamTripResponse}) {
 print(roamTripResponse?.toJson());
}, ({error}) {
 	 print(error?.toJson());
});
```

</div>

</div>

##Subscribe Trip

To subscribe to the real-time status of any ongoing trip, pass the `trip id` in the `subscribeTrip()` method.

<div class="code panel pdl" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` dart
Roam.subscribeTrip(tripId);
```

</div>

</div>

##Unsubscribe Trip

<div class="code panel pdl" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` dart
Roam.unsubscribeTrip(tripId);
```

</div>

</div>

