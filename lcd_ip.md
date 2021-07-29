# LCD_IP
This code will show ip on i2c lcd 16x2 display for Armbian on Orange Pi Zero
Pins: GND 5V SDA/PA12 SCK/PA11

## Setup
Activate i2c bus 0 (i2c0):
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
get i2c address of display from bus 0 (you should see 27):
```
sudo i2cdetect -y 0
```
check i2c_address and bus number in python code:
```
nano  lcd_ip.py
  I2C_ADDR  = 0x27
  bus = smbus.SMBus(0)
```
run
```
sudo python lcd_ip.py
```
press Ctrl+c to escape python.

## Start on boot:
open lcd.service and insert service code. check your path.
```
sudo nano /lib/systemd/system/lcd.service
 [Unit]
 Description=My LCD Service
 After=multi-user.target

 [Service]
 Type=simple
 ExecStart=/usr/bin/python /home/yourUserNameHere/lcd_ip.py > /home/yourUserNameHere/lcd.log 2>&1

 [Install]
 WantedBy=multi-user.target
```
set permission and start service: NOTE, IP CAN TAKE 60 SECONDS TO SHOW UP ON DISPLAY
```
sudo chmod 644 /lib/systemd/system/lcd.service
sudo systemctl daemon-reload
sudo systemctl enable lcd.service
sudo reboot
```
