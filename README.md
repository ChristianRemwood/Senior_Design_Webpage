## Welcome to the Air Quality Monitor Project Demo

## Project Developers

### [Jake Halopoff](https://github.com/kirmal-mal/sd-server/tree/main/views/pages)

### [Kirill Malevich](https://github.com/kirmal-mal)
Kirill Malevich was in charge of the server part of this project.

### [Christian Remwood](https://github.com/ChristianRemwood)
<img src ="https://instagram.fboi1-1.fna.fbcdn.net/v/t51.2885-15/sh0.08/e35/s640x640/120220428_171931904531356_5192354416253353930_n.jpg?_nc_ht=instagram.fboi1-1.fna.fbcdn.net&_nc_cat=104&_nc_ohc=XwSUQFCIlW8AX_4PN0T&tp=1&oh=8dd6a293b01e79100104b9c6a50c5258&oe=5FF71F0D" width="200" height="200" />

Christian Remwood was in charge of all hardware/firmware developments for this project. He has been working in the Firmware/Embedded sofrware field for about 7 years now and decided to put those skills to use in this semester. 

## The Project

### Overview
Our goal for this project was to create an Air Quality Monitor that could show you the quality of your homes air in real time over the internet. In order to do this, the project was broken up into three sections: Hardware, Application Server, and User Interface (Web Page). The general idea is that the hardware could be paired to some users account and ping the application server each time it measured the air quality. The server would then authenticate the request, make sure all credentials are valid, and then update any database entry where the sensor was tied to. The User could then view this data as past entries, or in real time by logging into their account and clicking on the sensor they wished to see. Over all it was a nice project giving us all experience creating distinc interfaces for having all parts work together. 

### Hardware

- ESP32 Feather (MAIN CPU/WIFI Radio)

<br />
<img src ="https://cdn-shop.adafruit.com/1200x900/3405-06.jpg" width="100" height="100" />
The ESP32 board was used as the main CPU running all the code responsible for the sensor code. Communications to the sensors was done through i2c, which was nice as it simplified wireing greatly. This board also handled both connecting to a users wifi, as well as hosting a wifi ap which could be used for initial setup. When a user connected to the sensors wifi access point, a captive portal would automatically be loaded with a form to pair the sensor with an account and connect it to a new wifi network. Lastly this sensor would automatically handle all communications with the application server from initial pairing, to updating the users data, and finally un-pairing if the user requested this to be done. 


<br />

- Si7021 (Temperature & Humidity)

<br />
<img src ="https://cdn-shop.adafruit.com/1200x900/3251-04.jpg" width="100" height="100" />
This sensor was used to get more accurate measurements from the Gas Sensor. It was able to provide accurate Temperature and Humidity data. 

<br />

- SGP30 (Gas Sensor)

<br />
<img src ="https://cdn-shop.adafruit.com/1200x900/3709-03.jpg" width="100" height="100" />
This sensor was the most important part of the project as it provided all the gas values we needed to use in order to determine local air quality. It was capable of providing raw ethonol, Volatile Organic Compund counts, CO2 PPM, and more. 

<br />

- Breadboard prototype (Connects components to CPU)

<br />
<img src ="https://media.discordapp.net/attachments/747919849220735017/784868204299550760/Screenshot_20201205-124536.png" width="100" height="100" />
<br />
<img src ="https://media.discordapp.net/attachments/747919849220735017/784867736651169833/PXL_20201123_232635134.jpg" width="100" height="100" />
When proofing out the hardware I started with connecting all components with a breadboard I had laying around. This was perfect for initial testing, but it did make for a source of error as wires could easily come loose. 

<br />

- Custom PCB (Connects components to CPU)

<br />
<img src ="https://media.discordapp.net/attachments/747919849220735017/784867614131617792/20201123_161811.jpg" width="100" height="100" />
<br />
<img src ="https://media.discordapp.net/attachments/747919849220735017/784867809095188510/PXL_20201123_233023795.jpg" width="100" height="100" />
In order to make for a more final product and remove the issues from the breadboard I created a custom PCB to connect all components together in a more secure manner. The board was designed using EAGLE CAD, and ordered from a company called OSH Park. It was not the most ideal layout, but I wanted to ensure the radio antenna had plenty of clearence to provide for the most stable wifi connection. Powering the board once assembled was more of an afterthough, but removing some spacers from the sensors gave enough room for a usb cable to connect to the ESP32 board. 

### Application Server
Application server is a Node.js server that accepts information from the devices and provides simple user interface to add a new device or view data recieved from the device.
The server has following functions:
- Helper methods for the user interface
  - Add the new user to the database
  - Authenticate user
  - Pair the new device
  - Return the list of devices for the user
  - Return the device readings
- Pair the new device
  - Create the pair token for the device
  - The API JSON for device pairing:<br/>
  
  ```
  {
    "type" : "pair", 
    "token": "%pairing_token_from_the_website", 
    "model": "%model_of_the_device%", 
    "serial": "%serial_number_for_the_device%"
  }
  ```
  
- Accept data from the device
  - The API JSON to post the logs is following:<br/>
  
  ```
  {
    "type": "postlogs",
    "token": "%token_for_the_paired_device",
    "tvoc": "%tvoc_reading%",
    "eco2": "%eco2_reading%",
    "raw_h2": "%raw_h2_reading%",
    "raw_ethanol": "%raw_ethanol_reading%"
  }
  ```
  
  - Add recieved logs to the database
### User Interface
<br />
<img src ="https://cdn.discordapp.com/attachments/784915386151731251/784997681255940116/unknown.png" />
<br />
<img src ="https://media.discordapp.net/attachments/784915386151731251/784998011142012988/unknown.png?width=266&height=338" />
The login/signup page for the project. Upon signing in, you are redirected to:
<br />
<img src ="https://media.discordapp.net/attachments/784915386151731251/785000719296102400/unknown.png" />

This displays the location of each device, the token to add a new device, and some information about the currently connected devices.

<img src = "https://cdn.discordapp.com/attachments/784915386151731251/785000796370108466/unknown.png" />

Pressing Add Device displays a new device token for pairing. Log out returns to the previous screen. 

Devices are shown with their ID, model, name, and date/time of the last recieved reading.  
<img src = "https://cdn.discordapp.com/attachments/784915386151731251/785000888234278912/unknown.png" />

## Product Usage Guide

### 1. Online Profile
  Currently the website is deployed on heroku by the address: [https://sd-env-logs-server.herokuapp.com/](https://sd-env-logs-server.herokuapp.com/)
  1. First user needs an account. To create one you need to enter the credentials into the register form:<br/>
  <img src ="https://cdn.discordapp.com/attachments/784915386151731251/785025213800120320/unknown.png" width="200" height="300" />
  2. After registration or if you already have an account, you can log in using following form:
  <img src ="https://cdn.discordapp.com/attachments/784915386151731251/785025478238797854/unknown.png" width="200" height="200" />
  3. Once you authorised you will see the follwing screen:
  <img src ="https://cdn.discordapp.com/attachments/784915386151731251/785028120850858024/unknown.png" width="600" height="200" />
  
### 2. Setting up Hardware 

1. First the user must power up the ESP32 over usb
2. Next they will connect to a netowrk AQSETUPXXXX where the XXXX would be part of the devices MAC address
3. The user will see a captive portal page load which they will need to enter their pairing code, wifi network, and wifi password. This info will be stored and the device will now reboot.
4. The setup should now be complete and the user will be able to see their device on the main application web page as long as they have access to the internet. 

### 3. Pairing the device
  1. To pair the device you need to press "add device" button on the account screen:
  <img src ="https://cdn.discordapp.com/attachments/784915386151731251/785025569615380520/unknown.png" width="400" height="200" />
  2. After you press the button, the server will give you an pairing token.
  <img src ="https://cdn.discordapp.com/attachments/784915386151731251/785025642441867274/unknown.png" width="400" height="200" />
  3. This token should be sent in JSON object from the device with the POST request in the following format:
  
  ```
  {
    "type" : "pair", 
    "token": "%pairing_token_from_the_website", 
    "model": "%model_of_the_device%", 
    "serial": "%serial_number_for_the_device%"
  }
  ```
  
  <br/>
  Python example:
  <img src ="https://cdn.discordapp.com/attachments/784915386151731251/785025805846708265/unknown.png" width="800" height="200" />
  4. As a response you will recieve a JSON object with authorisation token:<br/>
  
```
{
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJkZXZpY2VfaWQiOjMxLCJpYXQiOjE2MDcyMzYxMjl9.HlnK8NPzoY2RMk0swNlEyzlAoh2-04K6HPvZ_jUSqck"
}
```

### 4. Sending the data
1. To send the data you need to send a post request to the following address: https://sd-env-logs-server.herokuapp.com/postlogs
2. The JSON format included in the request is following:
```
  {
    "type": "postlogs",
    "token": "%token_for_the_paired_device",
    "tvoc": "%tvoc_reading%",
    "eco2": "%eco2_reading%",
    "raw_h2": "%raw_h2_reading%",
    "raw_ethanol": "%raw_ethanol_reading%"
  }
``` 
  <br/>
  Python example:
  <img src ="https://media.discordapp.net/attachments/784915386151731251/785026766912880680/unknown.png" width="800" height="150" />
3. To stop device from sending the data you should delete this device from the user by pressing "X" next to the device:
<img src ="https://cdn.discordapp.com/attachments/784915386151731251/785027053702348830/unknown.png" width="800" height="75" />

### 5. Viewing the data
1. To view the data you press the "view logs" button on the device string. You will see the following screen:
<img src ="https://cdn.discordapp.com/attachments/784915386151731251/785026937256673290/unknown.png" width="800" height="300" />
