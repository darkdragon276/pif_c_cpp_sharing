# Bluetooth Low Energy (BLE)

# Intro to BLE

## what is Bluetooth Low Energy(BLE)?

![Alt text](./assets/ble_overview.png)

- BLE is low-power wireless technology used for connecting devices with each other.
- BLE is targeted more towards application that need to consume less power.

## Stack's BLE

![Alt text](./assets/ble_stack.jpeg)

### Physical Layer(PHY)

- Bluetooth Low Energy shares some similarities with classic Bluetooth. Both use the 2.4 GHz band.
- There are 40 channels defined, each with a width of 2MHz.
  There are two types of channels:
  _ advertising channels (channel 37,38,39)
  _ data channels.(channel 0-36)
- The advertising channels are used for device discovery, broadcasting messages, and setting up connections.

![Alt text](./assets/ble_channel.png)

### Link Layer(LL)

- Handling the states of Advertising, Scanning, Create connection and maintenance.
- It is also responsible for the structure of the packets.

![Alt text](./assets/ble_linklayer.jpg)

- Standby: It does not transmit or receive packets, also known as the idle state.
- Scanning: It listens to advertising packets
- Initiating: It listens to advertising packets and responds to initiate a connection with another device.
- Connected: This state can be accessed from either the Scanning or Initiating state. Two roles are defined:
  - Master role: When accessed from Initiating state.
  - Slave role: When accessed from Advertising state.

**Example**:\
Device A (smartphone) acts as a `Scanner`. It listens to **advertising** packets from other BLE devices, including Device B (temperature sensor) acting as an `Advertiser`.

Device B (temperature sensor) acts as an `Advertiser. It broadcasts advertising packets to announce its presence and the services it provides.

After Device A (smartphone) discovers Device B (temperature sensor) through the scanning process, it can `initiate a connection` with Device B through the Master role. Once the connection is established, Device A (smartphone) will act as the `Master`, while Device B (temperature sensor) will act as the `Slave``.

# Generic Access Profile (GAP)

## GAP roles

### Broadvaster and observer

![Alt text](./assets/ble_Gap_role.png)

### Central and Peripheral

![Alt text](./assets/ble_Gap_role.png)

- GAP and Link layer differences

  - Each Gap role provides a specific set of requierments to the Link Layer.
  - Implementations of the rules which are laid out by the gap for a particular role is taken care of by the Link Layer.
    <br>

  ![Alt text](./assets/ble_gap_ll.png)

  - Link Layer advertising package include Gap package.
    <br>
    
  ![Alt text](./assets/ble_gap_ll_pk.png)

In short:

![Alt text](./assets/ble_gap_ll_end.)

## Advertising

- During the Advertising state, advertising packets are **periodically** sent on each of the three advertising channels
- The formula for the interval of time between sending advertising packets is:
  ` Interval Time = Fixed Interval + Random Delay`
  ![Alt text](./assets/ble_ad_time.png)
- This random delay helps reduce collisions between advertising packets from different devices

**note**:

- Fixed Interval, this value greatly impacts battery life and must be chosen very carefully
- it's recommended to choose the longest advertising interval that can still satisfy an acceptable user experience

Example:

```C
esp_ble_adv_params_t adv_params = {
    .adv_int_min = 0x20, // Minimum time between two advertising events (32 * 0.625 ms = 20 ms)
    .adv_int_max = 0x40, // Maximum time between two advertising events (64 * 0.625 ms = 40 ms)
    .adv_type = ESP_BLE_ADV_TYPE_IND, // Advertising type: non-connectable and scannable
    .own_addr_type = BLE_ADDR_TYPE_PUBLIC, // Public address of the current device
    .channel_map = ADV_CHNL_ALL, // Use all BLE channels for advertising
};
```

### Packet data structure

When sending packets, beacons can be configured to a variety of standards,
such as: iBeacon, Eddystone, AltBeacon.

![Alt text](./assets/ble_ad_pack.png)

Example:

- Raw Package

```C
static const uint8_t spp_adv_data[23] = {
    /* Flags */
    0x02,0x01,0x06,
    /* Complete List of 16-bit Service Class UUIDs */
    0x03,0x03,0xF0,0xAB,
    /* Complete Local Name in advertising */
    0x0F,0x09, 'E', 'S', 'P', '_', 'B', 'L', 'E', '_', 'S', 'E', 'R','V', 'E', 'R'
};
```

We use this function to set package advertising.

`esp_err_t esp_ble_gap_config_adv_data_raw(uint8_t *raw_data, uint32_t raw_data_len)`

- Ibeacon(Apple)

```C
typedef struct {
    uint8_t flags;        // Advertising flags, specifying the iBeacon state (usually 0x02).
    uint16_t company_id;  // Company ID (usually 0x004C for Apple Inc.).
    uint16_t beacon_type; // iBeacon type (usually 0x1502).
    uint8_t uuid[16];     // UUID of the iBeacon, with a length of 16 bytes.
    uint16_t major;       // Major number of the iBeacon.
    uint16_t minor;       // Minor number of the iBeacon.
    int8_t rssi;          // Received Signal Strength Indicator (RSSI) of the iBeacon.
} esp_ble_ibeacon_t;
```

## Scanning

There are two scan types:

- **Passive Scanning**: This type of passive scanning only receives Advertising packets.

![Alt text](./assets/passive-scanning.png)

- **Active Scanning**: This type of active scanning apart from receiving packets fromA dvertising, you can send response packets to request more information of the devices

![Alt text](./assets/active-scanning.png)

### Scan Interval

The scanning process depends on two parameters:

- Scanning window: Duration of listening to Advertising packets.
- Scan Interval: Frequency that the listening occurs.

![Alt text](./assets/ble_time_scan.png)

Example:

```C
//in ESP32
esp_ble_scan_params_t ble_scan_params = {
    .scan_type = BLE_SCAN_TYPE_ACTIVE, // Scan type: active or passive
    .scan_interval = 0x50, // Time between scan events (0x50 = 80ms)
    .scan_window = 0x30, // Time of scanning during each scan event (0x30 = 48ms)
};
```

### Packet data structure

![Alt text](./assets/ble_scan_pck.png)

This packet has two types of payload:

- `SCAN_REQ`: The link layer of the scanning device sends this packet, which is received by the Advertiser device that sent the Advertising packet. This packet does not contain host data.
- `SCAN_RSP`: The Advertiser device sends this packet in response to a SCAN_REQ packet. This packet can contain host data in the ScanRspData field.

Example:

```C
void configure_scan_response_data() {
    uint8_t scan_rsp_data[] = {
        0x02,  // Length of the Scan Response data
        0x0A,  // Service UUID (e.g., 0x0A is a Notification Service)
        0x09,  // Device Name
        'M', 'y', ' ', 'D', 'e', 'v', 'i', 'c', 'e'
    };
    esp_ble_gap_config_scan_rsp_data_raw(scan_rsp_data, sizeof(scan_rsp_data));
}

```

**Note**:
This packages are transmitted by advertiser (if we config scaner in Active mode)

# Generic Attribute Profile (GATT)
