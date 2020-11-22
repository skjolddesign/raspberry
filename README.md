# raspberry
i2c lcd 16x2 display for Armbian on Orange Pi Zero
Pins: GND 5V SDA SCK

install:
sudo armbian-config -> activate i2c0
sudo adduser myuser i2c
reboot
wget https://raw.githubusercontent.com/skjolddesign/raspberry/main/lcd_ip.py
sudo apt-get install -y i2c-tools python-smbus
sudo i2cdetect -y 0 (scans i2c0 for addresses, you should see ex 27)
nano  lcd_ip.py
  I2C_ADDR  = 0x27
  bus = smbus.SMBus(0)
adduser myuser i2c
python lcd_ip.py

Start on boot:
sudo nano /lib/systemd/system/lcd.service
 [Unit]
 Description=My LCD Service
 After=multi-user.target

 [Service]
 Type=simple
 ExecStart=/usr/bin/python /home/myuser/lcd_ip.py > /home/myuser/lcd.log 2>&1

 [Install]
 WantedBy=multi-user.target
 
sudo chmod 644 /lib/systemd/system/lcd.service
sudo systemctl daemon-reload
sudo systemctl enable lcd.service
sudo reboot
