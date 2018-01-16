## SDK

### Contents

1. [Introduction](#introduction)

2. [Authentication](#authentication)

3. [iOS](#ios)
   * [Configure project](#configure-project)
   * [Get the latest version of Xcode](#get-the-latest-version-of-xcode)
   * [Download the framework](#download-the-framework)
   * [Integrate the framework](#integrate-the-framework)
   * [Integrate SDK into app](#integrate-sdk-into-app)
      - [Import the SDK](#import-the-sdk)
      - [Create Location Service Scanner Manager](#create-location-service-scanner-manager)
      - [Start Location Services](#start-location-services)
      - [Stop Location Services](#stop-location-services)
      - [Subscribe to Segments or Places](#subscribe-to-segments-or-places-using-tags)
      - [Advanced framework configuration](#advanced-framework-configuration)
      - [Register for callbacks](#register-for-callbacks)
      - [Sample implementation](#sample-implementation)
      - [Submit to App Store](#submit-to-app-store)

4. [Support](#support)

### Introduction
Nikaza serves as a bridge between the physical and digital worlds. Integrate Nikaza Location Services Framework into your iOS app to start tracking users and generating events. The SDK allows you to add location context and tracking to your apps with only a few lines of code.

Nikaza Location Services Framework attempts to maximize battery efficiency. Most of the geofencing processing happens on the server-side. This allows Nikaza geofencing to be more powerful than native iOS geofencing with cross-platform support for unlimited geofences.

### Authentication

Authenticate using your unique API keys:

1. Contact [support@nikaza.io](mailto:support@nikaza.io) to obtain your unique API key.
2. Add the API key to the framework config file as mentioned in [Advanced framework configuration](#advanced-framework-configuration).

### iOS

#### Configure project

To track the user's location in the foreground, you must add a string for `NSLocationWhenInUseUsageDescription` key in your `Info.plist file`. This string will be displayed when prompting the user for foreground location permissions.

To track the user's location in the background, you must add a string for `NSLocationAlwaysUsageDescription` (iOS 10 and before) and `NSLocationAlwaysAndWhenInUseUsageDescription` (iOS 11 and later) keys in your Info.plist file. These strings will be displayed when prompting the user for background location permissions.

To scan the Eddystone beacons, you must add a string for the `NSBluetoothPeripheralUsageDescription` key in your Info.plist file. Eddystone scanning consumes more battery power so iBeacon is recommended.  

Then, in your project settings, go to `Capabilities > Background Modes` and turn on `Location updates, Background fetch and Uses Bluetooth LE accessories`.

To discover the Beacons in the background, you must add key  `bluetooth-central` in your Info.plist file.

To configure a per-domain exception so that your app can connect to a non-secure (or non TLSv1.2-enabled secure host), add `NSAppTransportSecurity` key to your Info.plist file.

For increased reliability and responsiveness, you can also turn on Location updates. Note that this requires additional justification during App Store review. Learn more [below](#submit-to-app-store).

```html
<key>NSBluetoothPeripheralUsageDescription</key>
<string>to scan the eddystone beacons</string>

<key>NSLocationAlwaysAndWhenInUseUsageDescription</key>
<string>Your iOS 11 and higher background location usage description goes here.</string>

<key>NSLocationAlwaysUsageDescription</key>
<string>Your iOS 10 and lower background location usage description goes here.</string>

<key>NSLocationWhenInUseUsageDescription</key>
<string>Your foreground location usage description goes here.</string>

<key>NSAppTransportSecurity</key>
<dict>
 <key>NSAllowsArbitraryLoads</key>
 <true/>
</dict>

<key>UIBackgroundModes</key>
<array>
  <string>fetch</string>
  <string>location</string>
  <string>bluetooth-central</string>
</array>
```

#### Get the latest version of Xcode

To build a project using the Nikaza Location Services Framework for iOS, you need version 6.3 or later of [Xcode](https://developer.apple.com/xcode/).

#### Download the framework

Nikaza Location Services Framework can be downloaded from [here.](https://github.com/NikazaInc/location-services-sdk/raw/master/ios/framework.zip)

#### Integrate the framework

1. Create an [Xcode](https://developer.apple.com/xcode/) project,If you do not have yet, and save it to your local machine. 
2. [Create a single view application](https://developer.apple.com/library/content/referencelibrary/GettingStarted/DevelopiOSAppsSwift/index.html#//apple_ref/doc/uid/TP40011343).
3. Copy and Paste the downloaded Nikaza Location Services Framework to your project directory.
4. Click on your project in the project explorer, then the General configuration page.
5. Add Nikaza Location Services Framework target to the `Embedded Binaries` section by clicking the `+` icon (Refer [Figure 1](#figure-1)).
6. Click on your project in the project explorer, then the `Build Phases` tab.
7. Add Nikaza Location Services Framework under `Link Binary With Libraries` section (Refer to [Figure 2](#figure-2)).


#### Figure 1

![Embedded Binaries](
images/location-services-embedded-bin.png)

#### Figure 2

![Link Binary With Libraries](
images/location-services-link.png)

#### Integrate SDK into app

#### Import the SDK

```objectivec
#import <NikazaLocationServices/NikazaLocationServices.h>
```
#### Create Location Service Scanner Manager

```objectivec
@property (nonatomic, strong) NikazaLocationServicesScannerManager *locationServicesScanner;

_locationServicesScanner = [[NikazaLocationServicesScannerManager alloc] initWithAPI_KEY:@"YOUR API KEY"];
```

#### Start Location Services

```objectivec
[_locationServicesScanner startLocationServicesFilterByTags:nil];
```
By default, the framework works only when the app is in the foreground. When the app goes to the background, the framework automatically stops working. Read [Configure project](#configure-project) to know more about the background modes.

By default, Nikaza Location Services Framework scans for iBeacons in the Nikaza network. You can activate Eddystones scanning if required. Users will be prompted to give permission for the app to use their location data.
Please note that Eddystone scanning consumes more battery power.

The app's Info.plist must include the `NSLocationWhenInUseUsageDescription` key with a short explanation of why Location Services is being used. See [Apple documentation](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW26) for details.


#### Stop Location Services

 ```objectivec
[_locationServicesScanner stopLocationServices];
```

#### Subscribe to segments or places using tags

 ```objectivec
NSArray *tags = [[NSArray alloc] initWithObjects:@"Starbucks", nil];
[_locationServicesScanner startLocationServicesFilterByTags:tags];
```
Nikaza Location Services Framework communicates with Nikaza server to determine if a user is at a place or a specific business category.

Tags works in the foreground and in the background.

Learn more about [Places tags](Places.md).

#### Advanced framework configuration

Take these optional steps to add or modify the Nikaza Location Services Framework settings.

Modify the default configuration values such as power saving mode, eddystone search, lost timeout and rssi range to control the frequency of Nikaza API calls. This is optional.
Example: If RSSI range is set to 20 then Nikaza API calls are trigerred only if the RSSI changes by at least 20dBm once a beacon is found. Lost timeout defines the amount of time to wait (after a beacon is lost) before trigerring an exit event to Nikaza backend. Change `POWER_SAVING_MODE = YES` to avoid the GPS polling and use only region monitoring. Note that, region monitoring is significantly less accurate.

 ```objectivec
_locationServicesScanner.powerSavingMode               = NO;
_locationServicesScanner.eddystoneSearch               = YES;
_locationServicesScanner.apiInvocation_RSSI_Difference = 20;
_locationServicesScanner.lostBeaconTimeout             = 20.0;
```
     
#### Register for callbacks

To listen for callback, implement `NikazaLocationServicesScannerManagerDelegate` in your class, then assign the `delegate` as self.

Implementation of the callbacks or delegates is optional.

 ```objectivec
@interface YourClassName () <NikazaLocationServicesScannerManagerDelegate>

_locationServicesScanner.delegate = self;
 ```
 
Below are the optional delegates or callbacks:

```objectivec
didFind_beacon
didFind_beaconURL
didUpdate_beacon
didLose_beacon
didEnter_region
didExit_region
didEnter_geofence
didExit_geofence
didGetLocationMetadata_nikaza
didUpdateStatus_framework
 ```
`didUpdateStatus_framework` callback provides the framework status. The status can be a success or error. Implement this delegate to enable the debug log.

### Sample implementation

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

// When an existing beacon is no longer seen (exited from beacon region), a ‘didLose_beacon:’ call gets triggered
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

// When entered into a geofence, a ‘didEnter_geofence:’ call gets triggered
- (void)locationScanner:(NikazaLocationServicesScannerManager *)scanner didEnter_geofence:(CLCircularRegion *)geofenceRegion {
    NSLog(@"Entered into geofence!: %@", geofenceRegion);
}

// When exited from a geofence, a ‘didExit_geofence:’ call gets triggered
- (void)locationScanner:(NikazaLocationServicesScannerManager *)scanner didExit_geofence:(CLCircularRegion *)geofenceRegion {
    NSLog(@"Exited geofence!: %@", geofenceRegion);
}

// If location metadata response is received from Nikaza server, `didGetLocationMetadata_nikaza:` call gets triggered.
// ‘locationInfo’ contains location metadata key-value pairs.
- (void)locationScanner:(NikazaLocationServicesScannerManager *)scanner didGetLocationMetadata_nikaza:(NSDictionary *)locationInfo Error:(NSError *)error {
    if (!error) {
        NSLog(@"Here are the Location details: %@", locationInfo);
    }
}

// When a framework status or error is available, a ‘didUpdateStatus_framework:’ call gets triggered.
// Implement this delegate to enable the debug log.
- (void)locationScanner:(NikazaLocationServicesScannerManager *)scanner didUpdateStatus_framework:(NSDictionary *)status Error:(NSError *)error {
    if (error) {
        NSLog(@"Error: %@",error.localizedDescription);
    } else {
        NSLog(@"Status: %@",[status objectForKey:NSLocalizedDescriptionKey]);
    }
}
 ```

### Submit to App Store

Apple requires that you justify your use of background location. Add something materially similar to the following to the bottom of your App Store description: 
```
This app uses background location to <insert use case here>. Continued use of background location may reduce battery life.
```

If you turned on the Location updates background mode, Apple requires additional justification. Add something materially similar to the following to your App Store review notes: 
```
This app uses Nikaza Location Services Framework to <insert use case here>. Nikaza Location Services Framework requires background location mode to support geofences and nearby place detection, which cannot be accomplished with region monitoring or visit monitoring.
```

Learn more about this requirement in section 2.5.4 of the App Store Review Guidelines [here](https://developer.apple.com/app-store/review/guidelines/#software-requirements)

For general development, we built this framework as a single dynamic library for all needed architectures so you can run on all your devices and the iOS Simulator without any changes. However, there’s one drawback to this approach - because they’re linked at runtime, when a dynamic library is compiled separately to the app it ends up in, it’s impossible to tell which architectures will actually be needed. Therefore, Xcode will just copy the whole thing into your application bundle at compile time. Other than the wasted disk space, there’s no real drawback to this in theory. In practice, however, iTunes Connect doesn’t allow adding unused binary slices.

The work around is to add the below Run Script to your build steps to remove the simulator architecture (x86_64 and i386) from your app during building process.

```objectivec
APP_PATH="${TARGET_BUILD_DIR}/${WRAPPER_NAME}"

# This script loops through the frameworks embedded in the application and
# removes unused architectures.
find "$APP_PATH" -name '*.framework' -type d | while read -r FRAMEWORK
do
FRAMEWORK_EXECUTABLE_NAME=$(defaults read "$FRAMEWORK/Info.plist" CFBundleExecutable)
FRAMEWORK_EXECUTABLE_PATH="$FRAMEWORK/$FRAMEWORK_EXECUTABLE_NAME"
echo "Executable is $FRAMEWORK_EXECUTABLE_PATH"

EXTRACTED_ARCHS=()

for ARCH in $ARCHS
do
echo "Extracting $ARCH from $FRAMEWORK_EXECUTABLE_NAME"
lipo -extract "$ARCH" "$FRAMEWORK_EXECUTABLE_PATH" -o "$FRAMEWORK_EXECUTABLE_PATH-$ARCH"
EXTRACTED_ARCHS+=("$FRAMEWORK_EXECUTABLE_PATH-$ARCH")
done

echo "Merging extracted architectures: ${ARCHS}"
lipo -o "$FRAMEWORK_EXECUTABLE_PATH-merged" -create "${EXTRACTED_ARCHS[@]}"
rm "${EXTRACTED_ARCHS[@]}"

echo "Replacing original executable with thinned version"
rm "$FRAMEWORK_EXECUTABLE_PATH"
mv "$FRAMEWORK_EXECUTABLE_PATH-merged" "$FRAMEWORK_EXECUTABLE_PATH"

done
```


### Support

Questions? We're here to help.

Email us at [support@nikaza.io](mailto:support@nikaza.io) and we’ll help you sort it out.
