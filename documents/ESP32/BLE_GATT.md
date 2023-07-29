# Bluetooth Low Energy (BLE)


# Intro to BLE
## what is Bluetooth Low Energy(BLE)?
![Alt text](./assets/ble_overview.png)

* BLE is low-power wireless technology used for connecting devices with each other.
* BLE is targeted more towards application that need to consume less power.

## Stack's BLE
![Alt text](./assets/ble_stack.jpeg)

### Physical Layer(PHY)
* Bluetooth Low Energy shares some similarities with classic Bluetooth. Both use the 2.4 GHz band.
* There are 40 channels defined, each with a width of 2MHz. 
There are two types of channels:
    * advertising channels (channel 37,38,39)
    * data channels.(channel 0-36)
* The advertising channels are used for device discovery, broadcasting messages, and setting up connections.

![Alt text](./assets/ble_channel.png)

### Link Layer(LL)
* Handling the states of Advertising, Scanning, Create connection and maintenance. 
* It is also responsible for the structure of the packets.

![Alt text](./assets/ble_linklayer.jpg)
* Standby:  It does not transmit or receive packets, also known as the idle state.
* Scanning: It listens to advertising packets
* Initiating: It listens to advertising packets and responds to initiate a connection with another device.
* Connected: This state can be accessed from either the Scanning or Initiating state. Two roles are defined:
    * Master role: When accessed from Initiating state.
    * Slave role: When accessed from Advertising state.

**Example**:\
Device A (smartphone) acts as a `Scanner`. It listens to **advertising** packets from other BLE devices, including Device B (temperature sensor) acting as an `Advertiser`.

Device B (temperature sensor) acts as an `Advertiser. It broadcasts advertising packets to announce its presence and the services it provides.

After Device A (smartphone) discovers Device B (temperature sensor) through the scanning process, it can `initiate a connection` with Device B through the Master role. Once the connection is established, Device A (smartphone) will act as the `Master`, while Device B (temperature sensor) will act as the `Slave``.
# Generic Access Profile (GAP)
## GAP roles


## Advertising

* During the Advertising state, advertising packets are **periodically** sent on each of the three advertising channels
* The formula for the interval of time between sending advertising packets is:
    ``` 
    Interval Time = Fixed Interval + Random Delay
    ```
![Alt text](./assets/ble_ad_time.png) 
* This random delay helps reduce collisions between advertising packets from different devices

**note**:
* Fixed Interval, this value greatly impacts battery life and must be chosen very carefully
*  it's recommended to choose the longest advertising interval that can still satisfy an acceptable user experience

### Packet data structure
When sending packets, beacons can be configured to a variety of standards,
such as: iBeacon, Eddystone, AltBeacon.

![Alt text](./assets/ble_ad_pack.png) 

## Scanning
There are two scan types:
* **Passive Scanning**: This type of passive scanning only receives Advertising packets.
* **Active Scanning**: This type of active scanning apart from receiving packets fromA dvertising, you can send response packets to request more information of the devices

### Scan Interval
The scanning process depends on two parameters:
* Scanning window: Duration of listening to Advertising packets.
* Scan Interval: Frequency that the listening occurs.

![Alt text](./assets/ble_time_scan.png)
### Packet data structure
![Alt text](./assets/ble_scan_pck.png)

This packet has two types of payload:
* `SCAN_REQ`: The link layer of the scanning device sends this packet, which is received by the Advertiser device that sent the Advertising packet. This packet does not contain host data.
* `SCAN_RSP`: The Advertiser device sends this packet in response to a SCAN_REQ packet. This packet can contain host data in the ScanRspData field.


# Generic Attribute Profile (GATT)