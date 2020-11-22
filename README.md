# Readme
This code will show ip on i2c lcd 16x2 display for Armbian on Orange Pi Zero
Pins: GND 5V SDA SCK

## Setup
Activate i2c:
```
sudo armbian-config
sudo adduser myuser i2c
reboot
```
get code and tools:
```
wget https://raw.githubusercontent.com/skjolddesign/raspberry/main/lcd_ip.py
sudo apt-get install -y i2c-tools python-smbus
```
get i2c address of display:
```
sudo i2cdetect -y 0 (scans i2c0 for addresses, you should see ex 27)
```
check i2c_address and bus number in python code:
```
nano  lcd_ip.py
  I2C_ADDR  = 0x27
  bus = smbus.SMBus(0)
```
run
```
python lcd_ip.py
```

Start on boot:
open lcd.service and insert service code. check your path.
```
sudo nano /lib/systemd/system/lcd.service
 [Unit]
 Description=My LCD Service
 After=multi-user.target

 [Service]
 Type=simple
 ExecStart=/usr/bin/python /home/myuser/lcd_ip.py > /home/myuser/lcd.log 2>&1

 [Install]
 WantedBy=multi-user.target
```
set permission and start service:
```
sudo chmod 644 /lib/systemd/system/lcd.service
sudo systemctl daemon-reload
sudo systemctl enable lcd.service
sudo reboot
```
