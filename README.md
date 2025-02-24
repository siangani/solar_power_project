Here's a well-structured **README.md** file for your project:  

---

# **Smart Solar Energy Management**  
### *Automated Diversion of Excess Solar Power to Home Heating Systems*  



## **📌 Overview**  
This project aims to **maximize solar self-consumption** by automatically detecting surplus solar energy and diverting it to existing electric heaters. Instead of exporting excess power to the grid (often with low or no financial return), the system **intelligently switches on home heaters**, ensuring efficient energy utilization and reduced electricity bills.  

### **🔧 Features**  
✅ **Real-time solar monitoring** via Enphase Envoy API  
✅ **Automated heater control** using Shelly smart relays  
✅ **Configurable prioritization** of primary & secondary heaters  
✅ **Live monitoring via smartphone app** (Blynk platform)  
✅ **Failsafe mechanisms** with Monit for system reliability  
✅ **Works with existing home heating** (no need for new hardware)  

---

## **🖥️ System Architecture**  

### **🔄 Workflow**  
1️⃣ **Monitor Solar Power & Grid Export**  
- Uses **Enphase Envoy** to track real-time energy flow.  
- Identifies when **excess energy is available**.  

2️⃣ **Activate Electric Heaters via Smart Relays**  
- Uses **Shelly 1PM or Shelly Plug S** to turn heaters on/off.  
- Supports **pilot wire control** for different heating modes.  

3️⃣ **Remote Control & Monitoring**  
- **Blynk app** allows users to **view energy flow** and manually control heaters.  
- Graphs show **solar production, home consumption, and heater usage**.  

4️⃣ **Failsafe & System Monitoring**  
- **Monit service** ensures the automation script runs continuously.  
- Auto-restarts processes if failures occur.  

---

## **📦 Hardware Requirements**  
| **Component**  | **Function** | **Model Used** |  
|---------------|-------------|--------------|  
| **Solar Monitor** | Tracks power flow | Enphase Envoy |  
| **Smart Relays** | Controls heaters via WiFi | Shelly 1PM, Shelly Plug S |  
| **Current/Voltage Sensor** | Measures diverted power | PZEM-004T |  
| **Single Board Computer** | Runs automation logic | Raspberry Pi (RPi 4 recommended) |  
| **Pilot Wire Control Circuit** | Controls heater modes | 230V relay with diode |  
| **Wireless Router** | Ensures network connectivity | Standard home WiFi router |  

---

## **🛠️ Software Requirements**  
| **Software**  | **Function** | **Installation** |  
|--------------|-------------|-----------------|  
| **Python 3** | Runs automation logic | `sudo apt install python3` |  
| **Blynk IoT** | Mobile app for control | `pip install blynk-library` |  
| **Shelly API** | Heater control | Local REST API |  
| **Enphase API** | Solar monitoring | Local REST API |  
| **Monit** | System monitoring | `sudo apt install monit` |  

---

## **🚀 Installation Guide**  
### **1️⃣ Set Up Raspberry Pi**  
Ensure your **Raspberry Pi is running Raspberry Pi OS** and connected to WiFi.  

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install python3 pip monit
```

### **2️⃣ Install Required Python Libraries**  
```bash
pip install requests blynklib
```

### **3️⃣ Configure Enphase Envoy API Access**  
- Get **local API access** from your **Enphase Envoy** device.  
- Store the API credentials in `config.json`.  

```json
{
  "envoy_ip": "192.168.X.X",
  "envoy_username": "your_username",
  "envoy_password": "your_password"
}
```

### **4️⃣ Connect Smart Relays (Shelly 1PM / Plug S)**  
- Install Shelly relays and **connect them to WiFi**.  
- Enable **local REST API** in the Shelly app.  
- Add relay IPs to `config.json`.  

```json
{
  "shelly_devices": {
    "heater1": "192.168.X.X",
    "heater2": "192.168.X.X"
  }
}
```

### **5️⃣ Configure & Start Automation Script**  
Run the Python script to **start monitoring & control**:  
```bash
python3 solar2heater.py
```

### **6️⃣ Enable Auto-Restart with Monit**  
```bash
sudo nano /etc/monitrc
```
Add the following:  
```plaintext
check process solar2heater matching "python3 solar2heater.py"
    start program = "/usr/bin/python3 /home/pi/solar2heater.py"
    if does not exist then restart
```
Restart Monit:  
```bash
sudo systemctl restart monit
```

---

## **📱 Smartphone App (Blynk Integration)**  
- Install the **Blynk IoT app** (Android/iOS).  
- Create a **new project** and add widgets for:  
  - **Live energy graphs**  
  - **Heater on/off control**  
  - **Status indicators**  
- Get the **Blynk Auth Token** and add it to `config.json`.  

```json
{
  "blynk_auth": "your_blynk_token"
}
```

---

## **🔍 Usage & Testing**  
1. **Check the logs for energy monitoring updates**  
   ```bash
   tail -f logs/solar2heater.log
   ```
2. **Test heater activation by simulating excess solar power**  
   ```bash
   python3 test_solar_input.py
   ```
3. **View real-time energy flow on the Blynk app**  

---

## **📊 Expected Results**  
✅ **Improved solar self-consumption** (less grid export).  
✅ **Lower electricity costs** by utilizing free solar energy for heating.  
✅ **Real-time monitoring & remote control** via a smartphone app.  
✅ **System reliability** with automatic recovery from failures.  

---

## **🛠️ Troubleshooting**  
| **Issue** | **Solution** |  
|-----------|------------|  
| No solar data received | Check Enphase API credentials & network |  
| Shelly relay not responding | Ensure Shelly device is on WiFi & API enabled |  
| Python script stops running | Check Monit logs & restart Monit |  

---

## **🔮 Future Improvements**  
- **Machine Learning Integration**: Predict solar availability & preemptively activate heaters.  
- **Voice Assistant Control**: Integrate with Google Assistant or Alexa.  
- **Battery Storage Support**: Hybrid approach for better energy efficiency.  

---

## **📜 License**  
This project is open-source under the **MIT License**.  

---

## **🙌 Acknowledgments**  
Special thanks to:  
- The **open-source community** for developing useful APIs.  
- Enphase, Shelly, and Blynk for their developer-friendly platforms.  

---

### **📩 Questions or Contributions?**  
👨‍💻 Feel free to **open an issue** or **submit a pull request** on GitHub!  

🚀 Happy Hacking!  

---

This README provides everything needed for setup, operation, and troubleshooting. You can modify the placeholders (e.g., API endpoints, image URLs) before uploading to GitHub. Let me know if you need any refinements! 🚀
