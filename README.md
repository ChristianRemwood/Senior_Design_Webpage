## Welcome to the Air Quality Monitor Project Demo

## Project Developers

### Jake Halopoff

### Kirill Malevich

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


- Si7021 (Temperature & Humidity)
<br />
<img src ="https://cdn-shop.adafruit.com/1200x900/3251-04.jpg" width="100" height="100" />
This sensor was used to get more accurate measurements from the Gas Sensor. It was able to provide accurate Temperature and Humidity data. 

- SGP30 (Gas Sensor)
<br />
<img src ="https://cdn-shop.adafruit.com/1200x900/3709-03.jpg" width="100" height="100" />
This sensor was the most important part of the project as it provided all the gas values we needed to use in order to determine local air quality. It was capable of providing raw ethonol, Volatile Organic Compund counts, CO2 PPM, and more. 

- Breadboard prototype (Connects components to CPU)
<br />
<img src ="https://media.discordapp.net/attachments/747919849220735017/784868204299550760/Screenshot_20201205-124536.png" width="100" height="100" />
<br />
<img src ="https://media.discordapp.net/attachments/747919849220735017/784867736651169833/PXL_20201123_232635134.jpg" width="100" height="100" />
When proofing out the hardware I started with connecting all components with a breadboard I had laying around. This was perfect for initial testing, but it did make for a source of error as wires could easily come loose. 

- Custom PCB (Connects components to CPU)
<br />
<img src ="https://media.discordapp.net/attachments/747919849220735017/784867614131617792/20201123_161811.jpg" width="100" height="100" />
<br />
<img src ="https://media.discordapp.net/attachments/747919849220735017/784867809095188510/PXL_20201123_233023795.jpg" width="100" height="100" />
In order to make for a more final product and remove the issues from the breadboard I created a custom PCB to connect all components together in a more secure manner. The board was designed using EAGLE CAD, and ordered from a company called OSH Park. It was not the most ideal layout, but I wanted to ensure the radio antenna had plenty of clearence to provide for the most stable wifi connection. Powering the board once assembled was more of an afterthough, but removing some spacers from the sensors gave enough room for a usb cable to connect to the ESP32 board. 

### Application Server

### User Interface

### Markdown

## Product Usage Guide

### Online Profile

### Setting up Hardware (Might be text only unfortunately)



Markdown is a lightweight and easy-to-use syntax for styling your writing. It includes conventions for

```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/ChristianRemwood/Senior_Design_Webpage/settings). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://docs.github.com/categories/github-pages-basics/) or [contact support](https://github.com/contact) and we’ll help you sort it out.
