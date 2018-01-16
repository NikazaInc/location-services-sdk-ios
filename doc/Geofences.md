## Geofences

### Contents

1. [Introduction](#introduction)

2. [Quickstart](#quickstart)

3. [How it works](#how-it-works)

4. [Examples](#examples)

5. [Support](#support)

### Introduction

A geofence is used to specify a custom geographical location, such as a coffee shop, an auto dealership or a retail outlet. If you want to monitor user activity in a certain location, you can create a geofence around it to be notified when an user enters or exits the geofence. You can deliver contextually relevant experience to your users this way.

With support for unlimited geofences, the Nikaza Location Services Framework is more powerful than the native iOS geofencing.

### Quickstart

1. Integrate the latest version of the Nikaza Location Services Framework into your app 
2. Create a virtual beacon (geofence) in Nikaza Context Hub.
3. Listen to entry and exit events via following callbacks

 * `didEnter_geofence` 
 * `didExit_geofence`

Learn more about [SDK implementation](SDK.md).

### How it works
* User creates a virtual beacon (geofence) in Nikaza context hub. Email us at support@nikaza.io if you do not have access to Nikaza context hub.
* Location meta-data (such as venue name, zone name, campaign URLs) is attached to the virtual beacon.
* When the SDK finds a matching geofence the user is notified and its metadata is retrieved. 


### Examples

For example, to get a callback when user enters or exits a geofence, use [Geofences](Geofences.md).

`_locationServicesScanner.delegate = self;`

 ```objectivec
// When a geofence is entered, a ‘didEnter_geofence:’ call gets triggered
- (void)locationScanner:(NikazaLocationServicesScannerManager *)scanner didEnter_geofence:(CLCircularRegion *)geofenceRegion {
    NSLog(@"Entered into geofence!: %@", geofenceRegion);
}

// When a geofence is exited, a ‘didExit_geofence:’ call gets triggered
- (void)locationScanner:(NikazaLocationServicesScannerManager *)scanner didExit_geofence:(CLCircularRegion *)geofenceRegion {
    NSLog(@"Exited geofence!: %@", geofenceRegion);
}
```
### Support

Questions? We're here to help.

Email us at [support@nikaza.io](mailto:support@nikaza.io) and we’ll help you sort it out.
