## Places

### Contents

1. [Introduction](#introduction)

2. [Quickstart](#quickstart)

3. [Examples](#examples)

4. [Support](#support)

### Introduction

The Nikaza Places database has millions of pre-defined locations that are categorized based on the location type which can be subscribed to.

Each place has a category, sub-category and location name. Example: Arts & Entertainment, Movie Theatre, Acme Cinema.

### Quickstart

1. Integrate the latest version of the Nikaza Location Services Framework into your app to know when a user visits a place.
2. Subscribe to location categories or location names using tags
3. Listen to entry and exit events via following callbacks

 * `didEnter_geofence`
 * `didExit_geofence`

Learn more about [SDK implementation](SDK.md).
Nikaza supports thousands of Tags. View the [full list of Tags](CSV/TagList.csv).

### Examples

```
NSArray *tags = [[NSArray alloc] initWithObjects:@"Starbucks", @"Honda", @"Red Lobster", nil];
[_locationServicesScanner startLocationServicesFilterByTags:tags];
```

To do something if a user is at a Starbucks Coffee Shop, use [Tags](Tags.md)

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

### Support

Questions? We're here to help.

Email us at [support@nikaza.io](mailto:support@nikaza.io) and we’ll help you sort it out.
