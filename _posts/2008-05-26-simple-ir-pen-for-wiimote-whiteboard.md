---
layout:     post
title:      "Simple IR Pen for Wiimote Whiteboard"
date:       2008-05-26 23:54:00
---

My previous foray into using a Wiimote with my laptop led me down the path of building a USB sensor bar so that I can use the Wiimote to control the pointer movements. This approach worked better than expected, but it doesn’t work so well if you want finer control of your mouse pointer. For example, when I was demonstrating the Wiimote integration with my laptop I was quite nervous about the demo not working, and this was made apparent by the shaky lines that I was drawing with the Mouse Gestures. As a result some of the Mouse Gestures did not register.

A better approach would be to do what [Johnny Lee](https://www.cs.cmu.edu/~johnny/) did with the Wiimote to create the [Wiimote Whiteboard](https://www.cs.cmu.edu/~johnny/projects/wii/). Johnny Lee used the Wiimote as an IR camera pointed at a projector screen, and created a pen with an LED which the Wiimote can track. This approach provides for more accuracy and smoother movements of the pointer.

The barrier of entry to the Wiimote Whiteboard is creating the IR pen. Johnny Lee suggests wiring up a circuit containing an IR LED, momentary switch, resistor, and power supply, then shoving it into a pen. If you google “IR pen” you will also come up with some complicated solutions. One guy even tried to cram the circuit into a highlighter casing.

![Image 1]({{ site.url }}/public/img/assets/2008/05/26/simple-ir-pen-for-wiimote-whiteboard/1.jpg)

My solution is really quite straightforward. In fact you only need to go to your local electronics store and pick up two items: an LED keyring torch; and an IR LED. When purchasing an LED keyring torch, make sure that you can easily replace the LED. I used this [LED keyring torch](https://jaycar.com.au/productView.asp?ID=ST3048&CATID=&keywords=led+torch&SPECIAL=&form=KEYWORD&ProdCodeOnly=&Keyword1=&Keyword2=&pageNumber=&priceMin=&priceMax=&SUBCATID=) from Jaycar Electronics. I then pulled the torch apart, pulled out the LED, and replaced it with an [IR LED](https://jaycar.com.au/productView.asp?ID=ZD1945&CATID=&keywords=infrared+LED&SPECIAL=&form=KEYWORD&ProdCodeOnly=&Keyword1=&Keyword2=&pageNumber=&priceMin=&priceMax=&SUBCATID=). This solution meant I didn’t have to do any soldering or fiddling around. It all fit together into a nice compact form factor that cost me less than $10, and took no longer than 10 minutes to switch the LED.

