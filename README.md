### Notes for installation

This project has been made with perl. Very simple to get started tracking your car with the cheap GPS306B china car tracking device.  
If anyone wants to clean up the code and make it more beautiful/readable, please send suggestions.
Please let me know if you can get this working - it's also great feedback if my documentation is good enough :)

### Device bought:
Not from here, but excatly the same device:
http://www.aliexpress.com/store/product/gps-Tracker-GPS306B-SMS-Real-time-tracking-2-4G-Attendance-management-Plug-Play-SOS-Alarm-Truck/530436_1889292868.html

### Install instructions
1. Setup device via SMS. See instructions further down.
2. Get a google API key for google maps javascipt. Here: https://console.developers.google.com/
3. Setup datebase in mysql by running sql script: gps.createdb
4. copy gps.config.sample to gps.config
5. Edit gps.config for your needs
6. Create log directory according to config.file
7. Run gps-tracker-server.
8. Setup a cron job that calls gps-mailday. This will send an email with trip-details daily.
   Example: gps-mailday $(date --date='yesterday' +"%Y-%m-%d") 123456789123456 user@domain.org

### Estimated GPRS usage:
* Logging every 30 sec
* Tracker packets : 130 Bytes
* OBD packets: 130 bytes
* Max usage/month = (130 bytes *2)*2*60*24*31 = 23 Megs/month. If driving all the time!

### GPS306B Setup via SMS:

```
begin123456 (Reset device, manual says, but it doesn't seem to be doing it)
password123456 XXXXXX (Set new password)
odoXXXXXX 3643 (Current ODO measurement from the cars dashboard )
time zoneXXXXXX 1 (UTC+1 for copenhagen time)

clearXXXXXX (erase SD-card, i think. Bad documentation)

apnXXXXXX internet (Telenor APN)
gprsXXXXXX,1,1 (TCP mode, server does need to reply, makes communication very reliable. If you want to use simple UDP-listener change to ,0,0)
adminipXXXXXX QQQ.RRR.SSS.TTT 11111 (my server and port 11111)
gprsXXXXXXX (Turn on GPRS mode)

obdiiXXXXXX 1 (Sends OBD info in both single and multi tracking, important if you want to record RPM,battery voltage and engine temperature)
less gprsXXXXXX on (Stops sending GPRS data 5 minutes after engine stops. Slamming a door or other shaking will wake it up)
fix030s***nXXXXXX (Records every 30 sec forever, live over GPRS)
save030s***nXXXXXX (Enables databuffering to SD-card when device without GPRS, also logged every 30 sek)
angleXXXXXX 010 (Makes a much smoother road-snapping when turning. This does not apply to SD-card data, only live data)
```

* If you want to wake up the device, you can send "checkXXXXXX" - it will make the device send data for the next 5 minutes.
* If you are totally lost and can´t find your car in a parking lot, send "fix030s001nXXXXXX", then you will get i SMS with coordinates and link to google maps :)

### Manual for the GPS306B Device.
A badly written manaul is included in this repository. You can also find it here:   
https://www.obd2-shop.eu/files/gps306a_b-user%20manual.pdf
