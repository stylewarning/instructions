# IBM S812L

## Advanced System Management

Information from [here](https://www.ibm.com/support/knowledgecenter/TI0003N/p8hby/browser.htm).

Connect Thunderbolt ethernet adapter, connect ethernet to HMC1.

Set local IP to `169.254.2.140` with subnet mask `255.255.255.0`.

Point browser to `https://169.254.2.147`.

Default username/password is `admin`/`admin`. You must change the password to access management features.

### Changed Settings

Under System Config > Firmware Config, change Firmware Type to OPAL.

Under System Config > Console Type, change console type to Serial.

### Changes Made to Networking

Two network interfaces set up:
- `enP1s9f0` (top port on right card) set to dhcp
  - Set `allow-hotplug`
- `enP3p5s0f0` (top port on left card) set to static
  - Address: `192.168.1.42`
  - Subnet mask: `255.255.255.0`
  - Gateway: `192.168.1.1`
  - Set `allow-hotplug`

Note: When changing settings in `/etc/network/interfaces`:
- `service networking restart`
- `ifup $interface`
  - If this says something like "error: file exists" then flush the device with `ip addr flush dev $interface` and try again.
