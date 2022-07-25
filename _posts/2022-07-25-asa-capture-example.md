---
layout: post
title: "ASA Capture Examples"
date: 2022-07-25 07:00:00 -0400
categories: [documentation]
tags: [cisco, network, asa, pcap, capture, homelab]
---

# ASA Capture Examples

<<<<<<< HEAD
## Capture Setup: Specify Source AND Destination
To setup a capture on an ASA I like to create an access-list that defines the traffic I'm interested in first. 
For the source and destination objects you can specify single hosts or entire subnets. 
Obviously the more specific you can be with your capture the easier it will be to read the output created. 
```bash
object-group network CAP-INT
 network-object host <SRC_IP>  AND/OR   network-object <SRC_SUBNET> <SRC_MASK>

object-group network CAP-EXT
 network-object host <DST_IP>  AND/OR   network-object <DST_SUBNET> <DST_MASK>

access-list CAP extended permit ip object-group CAP-INT object-group CAP-EXT
access-list CAP extended permit ip object-group CAP-EXT object-group CAP-INT
```

## Capture Setup: Specify only Source IP (or Destination)
Sometimes you wont know what both IPs are, you'll only have one IP. You can use the "any" keywords in their place.
```bash
object-group network CAP-INT
 network-object host <SRC_IP>  OR  network-object <SRC_SUBNET> <SRC_MASK>

access-list CAP extended permit ip object-group CAP-INT any4
access-list CAP extended permit ip any4 object-group CAP-INT
```

## Activate the capture
Next you need to start the capture and bind it to an interface. 
In this example "int outside" is defining the interface on my firewall I want to capture on.
CAP is the name of the access list we created above.
Circular-buffer will cause the ASA to constantly overwrite the buffer for the capture, dropping the oldest packet for newer once the buffer is full.
Real-time will display the capture results directly to the screen as well as log them to the buffer
```bash
cap CAP2 int outside access-list CAP circular-buffer real-time 
```

## Clean up
```bash
clear cap CAP2
no cap CAP2
```


## Download a copy of the captures
If the http server is enabled on the ASA you can easily download a copy of the captures by using the following URL constructs.
The /pcap at the end will format the file that is downloaded into a pcap format you can then open in Wireshark to review.
### SINGLE CONTEXT MODE
```bash
 https://<ip_of_asa>/admin/capture/<capture_name>/pcap

 ex: https://192.168.10.1/admin/capture/CAP/pcap
```

### MULTI CONTEXT MODE

```bash
 https://<ip_of_asa_admin_context>/admin/capture/<context name>/<capture_name>/pcap
 
 ex: https://192.168.20.1/admin/capture/ctx-prod/CAP/pcap
```
=======
## The following are capture examples
```bash

```
>>>>>>> 570fe669b19342cebf79f9b2f82c86a3866a844a
