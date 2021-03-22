# RTC-Call-Monitor
---
## About
RTC-Call-Monitor is a tool to detect an active UDP based voice/video call by:

* monitoring the rate of incoming/outgoing UDP packets from your machine's IP address.
* checking the source/destination address against known network blocks for the major voice/video conference providers.

When an active call is detected, will invoke a webhook for the start and end of a call.

### Supported Providers

- Zoom
- Microsoft Teams
- WebEx
- Slack
- Google Meet
  
## Getting Started

### Prerequisites

* [.NET 5.0 SDK](https://dotnet.microsoft.com/download/dotnet/5.0)

### Minimal Configuration
Before starting the application for the first time, modify the value for `LocalNetwork` in *appsettings.config* with the details of your local network.  The value is in [CIDR Notation](https://www.digitalocean.com/community/tutorials/understanding-ip-addresses-subnets-and-cidr-notation-for-networking). 

Most users will have a network configuration similar to the following:

```
  IP Address:  192.168.1.100
  Subnet Mask: 255.255.255.0
```
The configuration will be

`"LocalNetwork": "192.168.1.0/24"`

### Build/Run

RTC-Call-Monitor requires that your console has elevated privileges in order to monitor network traffic.

Windows: Open a new Powershell or command prompt using *Run as Administrator*
```
cd src
dotnet run
```

Linux/macOS:
```
cd src
sudo dotnet run
```

### Additional Configuration

#### Webhooks

All webhooks are HTTP POST requests and require a valid URI value.

**Call Start**

`CallStart` in *appsettings.config*

Body
```
{
  'provider': 'slack'
}
```

**Call End**

`CallEnd` in *appsettings.config*

Body
```
{
  'duration': 123
}
```
`'duration'` is the call length in seconds.

#### Adding new providers/network blocks

If the application is not detecting the start of your call, the most likely reason is that the IP address is not in a known network block for your provider.  To see the destination address of your call provider, enable debug logging.

In *appsettings.config:*
`"RtcCallMonitor": "Debug"`

In the console a message similar to the following will show in the console:

`Unmapped network 66.77.89.91 count 122`

Find your provider in *appsettings.config* and add a new entry for the network block

`66.77.0.0/16`

Restart the application and your call should be detected.

