# Mobile Apps Challenge - My Devices

My Devices is a web application that allows network devices (such as for example a Raspberry Pi) to be detected and claimed by the owner. Basic information about a network devices is stored in a database and made available through a Vue frontend application.

The applications consists of several components:

* the detection script: this detects the network devices and gathers some basic information as ip and mac address and posts the information to the backend.
* the backend: provisions an API that can be used by the detection script and the front end application.
* a database: that holds all the information about the users and their devices.
* a frontend application: allows the users to view and claim devices.

Most of these components are already build or being build as we speak.

Except for the frontend application. This will be your task.

Make sure to commit regularly.

## Creating the frontend application

Start by creating a new Vue application in this repository. Choose to `Manually select features` when you need to pick a preset.

Now also enable the `Router` component as shown in the screenshot:

![Router](./img/preset-features.png)

Next keep the defaults:

![Defaults](./img/defaults.png)

### Setup Vuetify

As a first package add vuetify to your App.

### Vue Router

Before starting on your journey you should watch this YouTube video about routing in Vue.js: [Vue: Routing For Dummies](https://www.youtube.com/watch?v=-uCUCmrNgeo). The require part is from the beginning till `14:18`.

## Create basics Views

Create the following views which will be needed later on. You don't have to add any functionality yet. Just display some fake information which can later be replaced by the real data.

* A register page that allows the user to enter:
  * a nickname
  * an email address
  * a password
* A login page
  * using an email address
  * a password
* A device page that displays the following information (examples provided):
  * the name of the device: `Thumper Control`
  * the type of the device: `Raspberry Pi Zero`
  * the IP address of the device: `10.0.1.23`
  * the MAC address of the device: `AA:BB:CC:DD:EE:FF`
  * the owner of the device: `Nico De Witte`
  * an image of the device: `https://opencircuit.nl/resources/content/90d45c2fda1c6/crop/900-600/Dagu-Wild-Thumper-6WD-all-terrain-chassis.-black-with-metallic-red-hubs..jpg`
  * The timestamp of when it was last seen: `10/14/2020, 8:10:12 PM`
* A device overview page. You can just leave this empty for the moment.

You can add some basic navigation for the moment as was shown in the YouTube video.

## Add Authentication

Follow this guide to add authentication: [https://github.com/BioBoost/mobile-apps-authentication-demo](https://github.com/BioBoost/mobile-apps-authentication-demo)

## Claiming Devices

Allow the user to claim a device as his/her own.

A device can be claimed by sending an **HTTP PATCH** to the `/devices/{id}/claim` route. The `{id}` should be the `id` of an existing device. No body data should be sent with the request. The device will be assigned to the current logged in user.

For example:

```text
PATCH http://localhost:8081/devices/2/claim
```

Response of the backend **(200 OK)**:

```json
{
  "id": 2,
  "name": "Telraam",
  "type": "Raspberry Pi",
  "description": "Counts the number of people passing by",
  "location": "2.85",
  "image": "https://www.radiozuidrand.be/wp-content/uploads/2019/10/header-image-2.jpg",
  "hostname": "telraam",
  "createdAt": "2020-11-18T14:15:08.000Z",
  "updatedAt": "2020-12-02T19:40:29.000Z",
  "User": {
    "id": 2,
    "firstname": "Nico",
    "lastname": "De Witte"
  },
  "DeviceInterfaces": [
    {
      "id": 2,
      "mac": "11:BB:CC:DD:EE:FF"
    }
  ],
  "IPReports": [
    {
      "id": 3,
      "ip": "168.23.32.1",
      "mac": "11:BB:CC:DD:EE:FF",
      "DeviceId": 2,
      "createdAt": "2020-11-18T14:15:08.000Z",
      "updatedAt": "2020-11-18T14:15:08.000Z"
    }
  ]
}
```

## Releasing Devices

Allow the user to release a device as his/her own.

A device can be released by sending an **HTTP PATCH** to the `/devices/{id}/release` route. The `{id}` should be the `id` of an existing device. No body data should be sent with the request. The current user will be removed as the owner of the device.

For example:

```text
PATCH http://localhost:8081/devices/2/release
```

Response of the backend **(200 OK)**:

```json
{
  "id": 2,
  "name": "Telraam",
  "type": "Raspberry Pi",
  "description": "Counts the number of people passing by",
  "location": "2.85",
  "image": "https://www.radiozuidrand.be/wp-content/uploads/2019/10/header-image-2.jpg",
  "hostname": "telraam",
  "createdAt": "2020-11-18T14:15:08.000Z",
  "updatedAt": "2020-12-02T19:43:53.000Z",
  "User": null,
  "DeviceInterfaces": [
    {
      "id": 2,
      "mac": "11:BB:CC:DD:EE:FF"
    }
  ],
  "IPReports": [
    {
      "id": 3,
      "ip": "168.23.32.1",
      "mac": "11:BB:CC:DD:EE:FF",
      "DeviceId": 2,
      "createdAt": "2020-11-18T14:15:08.000Z",
      "updatedAt": "2020-11-18T14:15:08.000Z"
    }
  ]
}
```

## Device Overview

A user should be able to view all devices in a device overview page. All devices can be retrieved using an **HTTP GET** request send to the route `http://localhost:8081/devices`.

For example:

```text
GET http://localhost:8081/devices
```

Response of the backend **(200 OK)**:

```json
[
  {
    "id": 1,
    "name": "WiFi Counter",
    "type": "Raspberry Pi",
    "image": "https://www.yetanotherblog.com/wp-content/uploads/2014/03/IMG_20140325_105251-300x267.jpg",
    "User": {
      "id": 1,
      "firstname": "John",
      "lastname": "Doe"
    },
    "DeviceInterfaces": [
      {
        "id": 1,
        "mac": "AA:BB:CC:DD:EE:FF"
      }
    ]
  },
  {
    "id": 2,
    "name": "Telraam",
    "type": "Raspberry Pi",
    "image": "https://www.radiozuidrand.be/wp-content/uploads/2019/10/header-image-2.jpg",
    "User": null,
    "DeviceInterfaces": [
      {
        "id": 2,
        "mac": "11:BB:CC:DD:EE:FF"
      }
    ]
  }
]
```

## Device Details

Allow a user to fetch the details of a device by for example clicking on a device component in the device overview. The device details can be fetched by sending an **HTTP GET** request to the route `http://localhost:8081/devices/{id}` where `{id}` is an existing device `id`.

For example:

```text
GET http://localhost:8081/devices/3
```

Response of the backend **(200 OK)**:

```json
{
  "id": 3,
  "name": "Smartphone Nico",
  "type": null,
  "description": null,
  "location": null,
  "image": null,
  "hostname": null,
  "createdAt": "2020-11-18T14:44:25.000Z",
  "updatedAt": "2020-11-18T14:44:25.000Z",
  "User": null,
  "DeviceInterfaces": [
    {
      "id": 3,
      "mac": "ee:9f:ff:ab:44:55"
    }
  ],
  "IPReports": [
    {
      "id": 13,
      "ip": "10.0.0.2",
      "mac": "ee:9f:ff:ab:44:55",
      "DeviceId": 3,
      "createdAt": "2020-12-02T19:39:26.000Z",
      "updatedAt": "2020-12-02T19:39:26.000Z"
    }
  ]
}
```

## Only Users Devices

The frontend should allow the user to filter his/her own devices. Fetch all devices from the `http://localhost:8081/devices` route and filter based on the `User`.

## Display Orphaned IP Reports

IP Reports are the results of the DHCP detector and are delivered to the backend.

Example of IP report:

```json
{
  "time": "Wed Dec 2 19:39:25 UTC 2020",
  "mac": "aa:bb:7a:ab:ab:ff",
  "ip": "10.0.0.2",
  "hostname": "unknown"
}
```

When delivered to the backend the database is checked if the `mac` address is known as a existing device. If not, the IP report is considered orphaned (not attached to a device).

Allow the user to view the orphaned IP reports.

These can be fetched from the API by sending an **HTTP GET** request to the route `http://localhost:8081/ipreports/orphaned`

For example:

```text
GET http://localhost:8081/ipreports/orphaned
```

Response of the backend **(200 OK)**:

```json
[
  {
    "id": 5,
    "ip": "168.23.32.1",
    "mac": "33:BB:CC:DD:EE:FF",
    "DeviceId": null,
    "createdAt": "2020-11-18T14:15:08.000Z",
    "updatedAt": "2020-11-18T14:15:08.000Z"
  },
  {
    "id": 4,
    "ip": "168.23.32.1",
    "mac": "22:BB:CC:DD:EE:FF",
    "DeviceId": null,
    "createdAt": "2020-11-18T14:15:08.000Z",
    "updatedAt": "2020-11-18T14:15:08.000Z"
  }
]
```

## Creating Devices

Allow the user of the frontend app to create new devices. This can be achieved by sending an **HTTP POST** request to the route `http://localhost:8081/devices`.

Include the following data in the body of the request:

```json
{
	"name": "Smartphone Nico",
	"interfaces": [
		{ "mac": "cc:33:7a:cc:ab:22" }
	]
}
```

Next to `name` and `interfaces`, you can also add `type`, `description`, `location`, `image` and `hostname`. All which are of type `string`.

For example:

```text
POST http://localhost:8081/devices
```

With the following body data:

```json
{
	"name": "Smartphone Nico",
	"interfaces": [
		{ "mac": "cc:dd:bb:aa:ab:ff" }
	]
}
```

Response of the backend **(201 CREATED)**:

```json
{
  "id": 8,
  "name": "Smartphone Nico",
  "DeviceInterfaces": [
    {
      "id": 12,
      "mac": "cc:dd:bb:aa:ab:ff",
      "DeviceId": 8,
      "updatedAt": "2020-11-18T14:44:25.581Z",
      "createdAt": "2020-11-18T14:44:25.581Z"
    }
  ],
  "updatedAt": "2020-11-18T14:44:25.508Z",
  "createdAt": "2020-11-18T14:44:25.508Z"
}
```
