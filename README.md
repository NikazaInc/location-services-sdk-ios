## Nikaza Location Services Framework

### Contents

1. [Introduction](#introduction)

2. [Components](#places)
   * [Places](#places)
   * [Geofences](#geofences)
   * [Beacons](#beacons)   
   * [Location Context](#location-context)

3. [Developer Tools](#developer-tools)
   * [SDK](#sdk)
   
4. [Examples](#examples)

5. [Support](#support)
   
### Introduction

Nikaza serves as a bridge between the physical and digital worlds. Nikaza Location Services Framework is the connective tissue that links physical world places to in-venue mobile experience. As developers wanting to incorporate location based contextual and personalized in-app experiences, you can use Nikaza Location Services Framework, add location context and tracking to your apps with a few lines of code.

### Components

#### Places
 
The Nikaza Places database has millions of pre-defined locations that are categorized based on the location type which can be subscribed to.

Each location has a category, sub-category and location name. Example: Arts & Entertainment, Movie Theatre, Acme Cinema.

Learn more about [Places](/ios/doc/Places.md).

#### Geofences

A geofence is used to specify a custom geographical location, such as a coffee shop, an auto dealership or a retail outlet. If you want to monitor user activity in a certain location, you can create a geofence around it to be notified when an user enters or exits the geofence. You can deliver contextually relevant experience to your users this way.

With support for unlimited geofences, the Nikaza Location Services Framework is more powerful than the native iOS geofencing.

Learn more about [Geofences](/ios/doc/Geofences.md).

#### Beacons

The Nikaza Location Services Framework scans for beacons (iBeacon and Eddystone). Beacons provide higher accuracy than geofences and can be used to identify custom geographical location, such as a coffee shop, an auto dealership or a retail outlet and specific zones such as entrance or cash counter within the geographical location.
 
Learn more about [Beacons](/ios/doc/Beacons.md).
 
#### Location Context

Nikaza Context Hub backend has a collection of location and context information associated with these locations. The server call to retrieve location context and associated location metadata will be triggered when the a Geofence, Beacon or WiFi event occurs. Developers can subscribe to context metadata via callbacks.

Learn more about [Location Context](/ios/doc/Location_Context.md).

### Developer Tools

You can integrate Nikaza Location Services Framework with your apps using our developer tools: the [SDK](/ios/doc/SDK.md).

### SDK

Integrate Nikaza Location Services Framework into your iOS app to start tracking users and generating events. You can use the Nikaza Location Services Framework, add location context and tracking to your apps with just a few lines of code.

Learn more about the [SDK](/ios/doc/SDK.md).

### Examples

You can use Nikaza Location Services Framework to power location context and tracking for any use case.

For example, to do something if a user is at a Starbucks Coffee Shop, using [Tags](/ios/doc/Tags.md)

 ```objectivec
 NSArray *tags = [[NSArray alloc] initWithObjects:@"Starbucks", nil];
 [_locationServicesScanner startLocationServicesFilterByTags:tags];
 
- (void)locationScanner:(NikazaLocationServicesScannerManager *)scanner didGetLocationMetadata_nikaza:(NSDictionary *)locationInfo Error:(NSError *)error {
    //user is at a Starbucks Coffee Shop
    if (!error) {
        NSLog(@"User is at a Starbucks Coffee Shop. Here are the location details: %@", locationInfo);
    }
}
```

To do something when location metadata response is received from the Nikaza server.

```objectivec
- (void)locationScanner:(NikazaLocationServicesScannerManager *)scanner didGetLocationMetadata_nikaza:(NSDictionary *)locationInfo Error:(NSError *)error {
    if (!error) {
        NSLog(@"Here are the location details: %@", locationInfo);
    }
}
```
### Support

Questions? We're here to help.

Email us at [support@nikaza.io](mailto:support@nikaza.io) and we’ll help you sort it out.
