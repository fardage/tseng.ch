+++
author = "Marvin Tseng"
date = 2021-12-31T09:31:54Z
description = ""
draft = false
image = "https://images.unsplash.com/photo-1536244636800-a3f74db0f3cf?crop=entropy&cs=tinysrgb&fit=max&fm=jpg&ixid=MnwxMTc3M3wwfDF8c2VhcmNofDkxfHxjbG91ZHxlbnwwfHx8fDE2NDEzODU3MzM&ixlib=rb-1.2.1&q=80&w=2000"
slug = "cloud-native-beacon-system"
title = "☁️ Cloud-Native Beacon System"

+++

We have built a cloud-native application for the SCAD module. Our application replaces the analogue doorbell in stores. Customers can install our iOS app on their smartphones. The app scans for BLE beacons and notifies the store owner when a customer enters the store.

{{< vimeo 661379495 >}}

The application is **attached to the physical world** since it utilizes smartphones and Bluetooth beacons to access real-world information and data. When a smartphone with our app encounters a registered beacon, it triggers an **event**. Our **cloud-native** and serverless backend architecture is built on Azure.

## Architecture

### Overview - Build for Resilience

![Overview](/scad-beacon-overview.png)

#### Functions

We use Azure Front Door to make our application highly available. We have created **two instances** of our  application that run in different Azure regions and created a configuration based on equal-weighted and same priority backends. This configuration **directs traffic to the nearest site** that runs the application. Azure Front Door continuously monitors the web application. The service provides **automatic failover** to the next available site when the nearest site is unavailable.

#### Cosmos DB

* The database has Availability Zones enabled to further improve availability and resiliency for our application.
* Periodic backups take full backups of all partitions from all containers under your account, with no synchronisation across partitions. The minimum backup interval is 1 hour.
* Global distribution


### Frontend for Store Owners

The front-end is a **web page** that can be accessed with a web browser. The user can **subscribe to events** from specific locations. Since our application is event-driven, we use **WebSockets** via Azure Web PubSub. This allows us to send notifications in real-time in our server-less environment.

![Notification](/scad-frontend-notification.png)

The Azure Function "negotiate" creates tokens for the frontend to access the Azure Web PubSub service. Another Azure Function called "notification" allows us to send messages through the **PubSub system**.

Unfortunately, we could not use the Azure Static Web Apps service. This would simplify the deployment by managing the frontend and APIs in one package. However, we ran into issues when using Azure Static Web Apps with extensions for Azure Web PubSub and also the "Bring your own Function"-button was not showing on portal.azure.com as documented.

### App for Store Customers

The decision to only do an iOS app and ignore Android is mainly the available knowledge in the team. Another reason is the deep integration of beacon monitoring iOS offers. However, this monitoring only works with beacons that support the [iBeacon protocol](https://developer.apple.com/ibeacon/). Luckily the Minew E9 Beacon supports this protocol. An iBeacon transmits its UUID, Major ID and Minor ID. We configured our beacons with different Major IDs. A Major ID represents a location or store.

![Mockup](/scad-walking-with-iphone-x-mockup.jpeg)

The CoreLocation framework of Apple was used. This allowed us to monitor all the beacons, even if the app is not running in the foreground. All Minew E9 Beacons have the same iBeacon UUID (E2C56DB5-DFFB-48D2-B060-D0F5A71096E0). If iOS detects a beacon with this UUID the app is started in the background for 10 seconds. Once the app is started in the background it monitors the distance between the iPhone and the iBeacon. This function is called every second. Once the distance is under a certain value, it triggers the gateway azure function.

## Try it out

* **Web App:**  [https://scad-frontdoor.azurefd.net/frontend](https://scad-frontdoor.azurefd.net/frontend)
* **iOS App**: [https://testflight.apple.com/join/N57TLQib](https://testflight.apple.com/join/N57TLQib)

