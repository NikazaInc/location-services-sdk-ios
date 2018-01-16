## Beacons

### Contents

1. [Introduction](#introduction)

2. [Quickstart](#quickstart)

3. [How it works](#how-it-works)

4. [Examples](#examples)

5. [Support](#support)


### Introduction

The Nikaza Location Services Framework scans for beacons (iBeacon and Eddystone). Beacons provide higher accuracy than geofences and can be used to identify custom geographical location, such as a coffee shop, an auto dealership or a retail outlet and specific zones such as entrance or cash counter within the geographical location.

### Quickstart

1. Integrate the latest version of the Nikaza Location Services Framework into your app to start scanning beacons.
2. Add a beacon in Nikaza Context Hub.
3. Listen to receive the following beacon events:

`_locationServicesScanner.delegate = self;`

 * `didFind_beacon` 
 * `didUpdate_beacon`
 * `didLose_beacon`
 * `didFind_beaconURL`
 * `didEnter_region`
 * `didExit_region`


Learn more about [SDK implementation](SDK.md).

### How it works

* User adds a beacon in Nikaza context hub. Email us at support@nikaza.io if you do not have access to Nikaza context hub.
* Location meta-data (such as venue name, zone name, campaign URLs) is attached to the beacon.
* When the SDK finds a matching beacon the user is notified and its metadata is retrieved.

iBeacon devices are detected via region monitoring. All you need to do is request the `always authorization` to access [Location Services](https://developer.apple.com/library/content/documentation/UserExperience/Conceptual/LocationAwarenessPG/CoreLocation/CoreLocation.html).

Eddystone beacon protocol is detected via [Core Bluetooth](https://developer.apple.com/library/content/documentation/NetworkingInternetWeb/Conceptual/CoreBluetooth_concepts/AboutCoreBluetooth/Introduction.html).

### Examples

For example, to get a callback when a beacon event occurs, use [Beacons](Beacons.md).

`_locationServicesScanner.delegate = self;`

 ```objectivec
// When a new beacon is found, a `didFind_beacon:` call gets triggered
- (void)locationScanner:(NikazaLocationServicesScannerManager *)scanner didFind_beacon:(id)beaconInfo {
    if ([beaconInfo isKindOfClass:[CLBeacon class]]) {
        NSLog(@"Saw iBeacon!: %@", beaconInfo);
    } else if ([beaconInfo isKindOfClass:[EddystoneBeaconInfo class]]) {
        NSLog(@"Saw Eddystone!: %@", beaconInfo);
    }
}

// If a beacon is broadcasting URLs, `didFind_beaconURL:` is triggered.
- (void)locationScanner:(NikazaLocationServicesScannerManager *)scanner didFind_beaconURL:(NSURL *)url {
    NSLog(@"Find a URL!: %@", [url absoluteString]);
}

// When an existing beacon is updated (RSSI change), a `didUpdate_beacon:` call gets triggered
- (void)locationScanner:(NikazaLocationServicesScannerManager *)scanner didUpdate_beacon:(id)beaconInfo {
    if ([beaconInfo isKindOfClass:[CLBeacon class]]) {
        NSLog(@"Update iBeacon!: %@", beaconInfo);
    } else if ([beaconInfo isKindOfClass:[EddystoneBeaconInfo class]]) {
        NSLog(@"Update Eddystone!: %@", beaconInfo);
    }
}

// When an existing beacon is no longer seen (exit from a beacon region), a ‘didLose_beacon:’ call gets triggered
- (void)locationScanner:(NikazaLocationServicesScannerManager *)scanner didLose_beacon:(id)beaconInfo {
    if ([beaconInfo isKindOfClass:[CLBeacon class]]) {
        NSLog(@"Lost iBeacon!: %@", beaconInfo);
    } else if ([beaconInfo isKindOfClass:[EddystoneBeaconInfo class]]) {
        NSLog(@"Lost Eddystone!: %@", beaconInfo);
    }
}

// Invoked when the user enters a monitored region.
- (void)locationScanner:(CLLocationManager *)manager didEnter_region:(CLRegion *)region {
    NSLog(@"Entered iBeacon Region!: %@", region);
}

// Invoked when the user exits a monitored region.
- (void)locationScanner:(CLLocationManager *)manager didExit_region:(CLRegion *)region {
     NSLog(@"Exited iBeacon Region!: %@", region);
}
```
### Support

Questions? We're here to help.

Email us at [support@nikaza.io](mailto:support@nikaza.io) and we’ll help you sort it out.
