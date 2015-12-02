---
title: IP-per-task
---

# IP-per-task

*EXPERIMENTAL* This feature is currently experimental and the API is still subject to changes. We appreciate
[your feedback](https://github.com/mesosphere/marathon/issues/2709)!

In version 0.14, Marathon introduced experimental IP-per-task support. With the appropriate configuration,
every tasks of an app gets its own network interface and IP address. This can drastically simplify 
service discovery with DNS since you can use address-only (A) DNS records in combination with known ports
to connect to your service -- as you would do it in a traditional static cluster environment.

You can request an IP-per-task with default settings like this:

```javascript
{
  "id": "/i-have-my-own-ip",
  // ... more settings ...
  "ipAddress": {}
}
```

Marathon passes down the request for the IP to Mesos. You have to make sure that you
installed & configured the appropriate 
[Network Isolation Modules](https://docs.google.com/document/d/17mXtAmdAXcNBwp_JfrxmZcQrs7EO6ancSbejrqjLQ0g/edit) & 
IP Access Manager (IPAM) modules in Mesos. 
The Marathon support for this feature requires Mesos 0.26.

Currently, this feature does not work in combination with docker containers. We might still change some
aspects of the API and we appreciate your feedback.

## Network security groups

If your IP Access Manager (IPAM) supports it, you can refine your IP configuration using network security
groups and labels:

```javascript
{
  "id": "/i-have-my-own-ip",
  // ... more settings ...
  "ipAddress": {
    "groups": ["production"],
    "labels": {
      "some-meaningful-config": "potentially interpreted by the IPAM"
    }
  }
}
```

Network security groups only allow network traffic between tasks that have at least one of their configured
groups in common. This makes it easy to disallow your staging environment to interfere with production
 traffic. 
