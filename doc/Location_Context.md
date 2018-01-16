## Location Context.

### Contents

1. [Introduction](#introduction)

2. [Quickstart](#quickstart)

3. [Examples](#examples)

4. [Support](#support)


### Introduction

Nikaza Context Hub backend has a collection of location and context information associated with these locations. The server call to retrieve location context and associated location metadata will be triggered when the a Geofence, Beacon or WiFi  event occurs. Developers can subscribe to context metadata via callbacks.

### Quickstart

1. Integrate the latest version of the Nikaza Location Services Framework into your app
2. Add a Geofence, Beacon or WiFi in Nikaza Context Hub.
3. Listen to receive the following event

  * `didGetLocationMetadata_nikaza` 

Learn more about [SDK implementation](SDK.md).

### Examples

To do something when location metadata response is received from the Nikaza server, use [Location Context](Location_Context.md).

`_locationServicesScanner.delegate = self;`

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
