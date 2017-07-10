# SSDPClient ![](https://img.shields.io/badge/swift-3.0-orange.svg) ![](https://img.shields.io/badge/plateforms-iOS%20%7C%20mac%20OS%20%7C%20linux-lightgrey.svg) ![GitHub license](https://img.shields.io/badge/license-MIT-blue.svg) [![GitHub release](https://img.shields.io/badge/version-v0.1.0-brightgreen.svg)](https://github.com/pierrickrouxel/SSDPClient/releases) ![Github stable](https://img.shields.io/badge/stable-true-brightgreen.svg)

[SSDP](https://en.wikipedia.org/wiki/Simple_Service_Discovery_Protocol) client for Swift using the Swift Package Manager. Works on iOS, macOS, and Linux.

## Installation
[![GitHub spm](https://img.shields.io/badge/spm-supported-brightgreen.svg)](https://swift.org/package-manager/)

SSDPClient is available through [Swift Package Manager](https://swift.org/package-manager/). To install it, add the following line to your `Package.swift` dependencies:

```swift
.Package(url: "https://github.com/pierrickrouxel/SSDPClient.git", majorVersion: 0, minor: 1)
```

## Usage
SSDPClient can be used to discover SSDP devices and services :

```swift
import SSDPClient

class ServiceDiscovery {
    let client = SSDPDiscovery()

    init() {
        self.client.delegate = self
        self.client.discover()
    }
}
```

To handle the discovery implement the `SSDPDiscoveryDelegate` protocol :

```swift
extension ServiceDiscovery: SSDPDiscoveryDelegate {
    func ssdpDiscovery(_: SSDPDiscovery, didDiscoverService: SSDPService) {
        // ...
    }
}
```

### Discovery
`SSDPDiscovery` provides two instance methods to discover services :

* `discoverService(type: String = "ssdp:all", timeout seconds: TimeInterval = 10)` - Discover SSDP services for a duration.

* `stop()` - Stop the discovery before the timeout.

### Delegate
The `SSDPDiscoveryDelegate` protocol defines delegate methods that you should implement when using `SSDPDiscovery` discover tasks :

* `func ssdpDiscovery(_ discovery: SSDPDiscovery, didDiscoverService service: SSDPService)` - Tells the delegate a requested service has been discovered.

* `func ssdpDiscovery(_ discovery: SSDPDiscovery, didFinishWithError error: Error)` - Tells the delegate that the discovery ended due to an error.

* `func ssdpDiscoveryDidStart(_ discovery: SSDPDiscovery)` - Tells the delegate that the discovery has started.

* `func ssdpDiscoveryDidFinish(_ discovery: SSDPDiscovery)` - Tells the delegate that the discovery has finished.

### Service
`SSDPService` is the discovered service. It contains the following attributes :

* `location: String?` - The value of `LOCATION` header
* `server: String?` - The value of `SERVER` header
* `searchTarget: String?` - The value of `ST` header
* `uniqueServiceName: String?` - The value of `USN` header
