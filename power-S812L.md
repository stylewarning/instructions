## Advanced System Management

Information from [here](https://www.ibm.com/support/knowledgecenter/TI0003N/p8hby/browser.htm).

Connect Thunderbolt ethernet adapter, connect ethernet to HMC1.

Set local IP to `169.254.2.140` with subnet mask `255.255.255.0`.

Point browser to `https://169.254.2.147`.

Default username/password is `admin`/`admin`. You must change the password to access management features.

### Changed Settings

Under System Config > Firmware Config, change Firmware Type to OPAL.

Under System Config > Console Type, change console type to Serial.