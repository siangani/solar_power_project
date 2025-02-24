Smart Solar Energy Management
Automated Diversion of Excess Solar Power to Home Heating Systems
(Optional: Add an image showcasing the system setup)

📌 Overview
This project aims to maximize solar self-consumption by automatically detecting surplus solar energy and diverting it to existing electric heaters. Instead of exporting excess power to the grid (often with low or no financial return), the system intelligently switches on home heaters, ensuring efficient energy utilization and reduced electricity bills.

🔧 Features
✅ Real-time solar monitoring via Enphase Envoy API
✅ Automated heater control using Shelly smart relays
✅ Configurable prioritization of primary & secondary heaters
✅ Live monitoring via smartphone app (Blynk platform)
✅ Failsafe mechanisms with Monit for system reliability
✅ Works with existing home heating (no need for new hardware)

🖥️ System Architecture
🔄 Workflow
1️⃣ Monitor Solar Power & Grid Export

Uses Enphase Envoy to track real-time energy flow.
Identifies when excess energy is available.
2️⃣ Activate Electric Heaters via Smart Relays

Uses Shelly 1PM or Shelly Plug S to turn heaters on/off.
Supports pilot wire control for different heating modes.
3️⃣ Remote Control & Monitoring

Blynk app allows users to view energy flow and manually control heaters.
Graphs show solar production, home consumption, and heater usage.
4️⃣ Failsafe & System Monitoring

Monit service ensures the automation script runs continuously.
Auto-restarts processes if failures occur.

🚀 Installation Guide
1️⃣ Set Up Raspberry Pi
Ensure your Raspberry Pi is running Raspberry Pi OS and connected to WiFi.

sudo apt update && sudo apt upgrade -y
sudo apt install python3 pip monit
2️⃣ Install Required Python Libraries

pip install requests blynklib
3️⃣ Configure Enphase Envoy API Access
Get local API access from your Enphase Envoy device.
Store the API credentials in config.json.

{
  "envoy_ip": "192.168.X.X",
  "envoy_username": "your_username",
  "envoy_password": "your_password"
}
4️⃣ Connect Smart Relays (Shelly 1PM / Plug S)
Install Shelly relays and connect them to WiFi.
Enable local REST API in the Shelly app.
Add relay IPs to config.json.

{
  "shelly_devices": {
    "heater1": "192.168.X.X",
    "heater2": "192.168.X.X"
  }
}
5️⃣ Configure & Start Automation Script
Run the Python script to start monitoring & control:

python3 solar2heater.py
6️⃣ Enable Auto-Restart with Monit

sudo nano /etc/monitrc
Add the following:

plaintext
Copy
Edit
check process solar2heater matching "python3 solar2heater.py"
    start program = "/usr/bin/python3 /home/pi/solar2heater.py"
    if does not exist then restart
Restart Monit:

sudo systemctl restart monit
📱 Smartphone App (Blynk Integration)
Install the Blynk IoT app (Android/iOS).
Create a new project and add widgets for:
Live energy graphs
Heater on/off control
Status indicators
Get the Blynk Auth Token and add it to config.json.

{
  "blynk_auth": "your_blynk_token"
}
🔍 Usage & Testing
Check the logs for energy monitoring updates

tail -f logs/solar2heater.log
Test heater activation by simulating excess solar power
bash
Copy
Edit
python3 test_solar_input.py
View real-time energy flow on the Blynk app
