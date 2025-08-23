# bop

## Project Overview

**Started:** August 11, 2025  
**Finished:** Aug 23, 2025  
**Total Logs:** 19  
**Time Invested:** 17 hours 29 minutes  
**GitHub:** [https://github.com/ShuchirJ/bop](https://github.com/ShuchirJ/bop)  

### Description

A tiny bluetooth 4.2 audio receiver to 3.5mm line level and headphone level out

![Project Cover](http://groundplane.hackclub.com/default_cover.png)

## Table of Contents

1. [Initial research! (2025-08-11)](#initial-research!-2025-08-11)
2. [Start w/ USB Circuit (2025-08-12)](#start-w/-usb-circuit-2025-08-12)
3. [Voltage Regulation, USB <-> UART (2025-08-12)](#voltage-regulation,-usb-<->-uart-2025-08-12)
4. [ESP + Buttons (2025-08-13)](#esp-+-buttons-2025-08-13)
5. [DAC!! (2025-08-13)](#dac!!-2025-08-13)
6. [Headphone Amp: Part I (2025-08-14)](#headphone-amp:-part-i-2025-08-14)
7. [Headphone Amp: Part II (2025-08-14)](#headphone-amp:-part-ii-2025-08-14)
8. [Cleaning up the schematic (2025-08-15)](#cleaning-up-the-schematic-2025-08-15)
9. [Cleaning up the schematic part II (2025-08-15)](#cleaning-up-the-schematic-part-ii-2025-08-15)
10. [DTR and RTS pins on UART (2025-08-16)](#dtr-and-rts-pins-on-uart-2025-08-16)
11. [Start PCB (2025-08-16)](#start-pcb-2025-08-16)
12. [pcb: lay out components (2025-08-16)](#pcb:-lay-out-components-2025-08-16)
13. [routing: part I (2025-08-16)](#routing:-part-i-2025-08-16)
14. [routing part II (2025-08-16)](#routing-part-ii-2025-08-16)
15. [silkscreen, cleaning things up (2025-08-17)](#silkscreen,-cleaning-things-up-2025-08-17)
16. [re-placing all parts (2025-08-18)](#re-placing-all-parts-2025-08-18)
17. [finished routing! (2025-08-18)](#finished-routing!-2025-08-18)
18. [case (2025-08-19)](#case-2025-08-19)
19. [cleanup (2025-08-23)](#cleanup-2025-08-23)

## Development Logs

### Initial research! - August 11, 2025 <a id="initial-research!-2025-08-11"></a>

**Time Spent:** 3h 0m  

#### What I Did

I've looked at many different things:
1. Making a custom ESP devboard: https://www.instructables.com/Build-Custom-ESP32-Boards-From-Scratch-the-Complet/
I'll be using this + an ESP32-WROOM-32E to use the raw module + my own usb-c implementation.
2. https://github.com/pschatzmann/ESP32-A2DP - I should be able to use this for the A2DP sink on the ESP32. 
3. I found my DAC from here: https://itohi.com/acoustics/esp32-as-bluetooth-audio
The PCM5102. JLCPCB only has the raw chip (PCM5102APWR) but I found a typical application circuit in the datasheet (https://www.ti.com/lit/ds/symlink/pcm5102a.pdf) which I should be able to use to route ESP32 to line level out
4. I'll need to add AC coupling near the jacks: https://www.youtube.com/watch?v=SsSlbno9G44
5. I asked a question on reddit to figure out what line level / headphone level is. https://www.reddit.com/r/diysound/comments/1mnfglu/confusion_with_line_level_vs_speaker_level/
I think I'll be including 2 jacks on the board so I have access to hifi audio but can also use earbuds if I want to. 

#### Issues Faced

what do all the levels mean 😭 is my car line level 😭 figured them out after quite a while of research

#### Next Steps

Going to see how to add an amp for the headphone out line + start a schematic on JLC.

#### Media

![Log Media](https://hc-cdn.hel1.your-objectstorage.com/s/v3/67d571de778aef8f4a756d17e11b6c40d34b665b_tmptflc5l_m.png)

---

### Start w/ USB Circuit - August 12, 2025 <a id="start-w/-usb-circuit-2025-08-12"></a>

**Time Spent:** 30m  

#### What I Did

got a little excited and decided to start on the schematic w/ what I can. The USB-C circuitry is done!

#### Next Steps

Add a voltage regulator to VCC_5V and start the custom ESP implementation.

#### Media

![Log Media](https://hc-cdn.hel1.your-objectstorage.com/s/v3/9b1737dd5e71867dfc544cfda7e0a8f04287d948_tmp4l3ic3p9.png)

---

### Voltage Regulation, USB <-> UART - August 12, 2025 <a id="voltage-regulation,-usb-<->-uart-2025-08-12"></a>

**Time Spent:** 1h 0m  

#### What I Did

Voltage regulation was easy, mainly because I've used an AMS1117 in a past project too so I was somewhat familiar. As I was about to start the ESP32 circuit I realized the ESP32-WROOM-32E doesn't support data over USB. That was a little frustrating, so what was like 20 ish min of work became an hour because I had to research USB to UART components. Found the CP2102 and copied over the typical application, so now I have a little onboard USB to UART interface. 

#### Issues Faced

didn't realize the wroom doesnt support usb, had to research alternatives

#### Next Steps

start esp circuitry!!

#### Media

![Log Media](https://hc-cdn.hel1.your-objectstorage.com/s/v3/800b1a02e34adcd77465e9edfca8c8a4d64bb064_tmpe93ti3ds.png)

---

### ESP + Buttons - August 13, 2025 <a id="esp-+-buttons-2025-08-13"></a>

**Time Spent:** 43m  

#### What I Did

I've added the main esp circuit + boot/rst buttons! This one took a little while to get started because the typical application circuit confused me. It took me a while to realize the big complicated parts were an RTC which I don't really need, so skipping that i just had to deal with UART (already done), boot, and reset pull ups.

#### Next Steps

start the PCM audio module circuit, try hooking up the esp to the pcm module

#### Media

![Log Media](https://hc-cdn.hel1.your-objectstorage.com/s/v3/10e95de897c77edc81634d9a6aec6b7205fed732_tmpdidyvhy7.png)

---

### DAC!! - August 13, 2025 <a id="dac!!-2025-08-13"></a>

**Time Spent:** 1h 0m  

#### What I Did

I think the hardest part, the DAC, is done now. Followed the typical app on the datasheet and now I have everything hooked up w/ two line outs. This one took me a little bit! I also researched RCA connectors a little, because I realized this might produce decent HiFi audio so I want to be able to have stereo output instead of just 3.5mm jacks. That was a little tricky because the pins on the RCA connector are just labelled 1 and 2, so I had to get a 3d view + pcb footprint to figure out which is gnd and which is signal.

#### Issues Faced

rca connector data sheet sucks

#### Next Steps

work on either the outputs or the headphone amp

#### Media

![Log Media](https://hc-cdn.hel1.your-objectstorage.com/s/v3/267678469113317b8464cc2c1726d15dc0c5883a_tmpcrahf45p.png)

---

### Headphone Amp: Part I - August 14, 2025 <a id="headphone-amp:-part-i-2025-08-14"></a>

**Time Spent:** 30m  

#### What I Did

the headphone amp part does not look fun...I've found the TPA6138A2 which seems to be relatively cheap while still being fully functional. The typical application looks like there's quite a lot involved though; I think some parts of this will definitely not be used i.e. the UVP/undervolt protection (mostly because I shouldn't need it). The main input receiving part seems not fun in the sense that there's a lot w/ configuring the RC circuits because certain configs change the output cutoff freqs. will have to see tomorrow!

#### Next Steps

gonna start implementing the headphone amp tmr

#### Media

![Log Media](https://hc-cdn.hel1.your-objectstorage.com/s/v3/0aa062b8ed090459e8798005e33683d6eeecdb2c_tmp0ztnkbe_.png)

---

### Headphone Amp: Part II - August 14, 2025 <a id="headphone-amp:-part-ii-2025-08-14"></a>

**Time Spent:** 1h 0m  

#### What I Did

put the headphone amp in the schematic!! it wasn't as daunting as it looked. had to do some calculations by hand which were not fun to calculate input capacitor + UVP protection resistor values but I got it done.

#### Next Steps

wire the outputs!

#### Media

![Log Media](https://hc-cdn.hel1.your-objectstorage.com/s/v3/899e1034145cff61e19e4842eb243b5823a8e79a_tmpbryo9iut.png)

---

### Cleaning up the schematic - August 15, 2025 <a id="cleaning-up-the-schematic-2025-08-15"></a>

**Time Spent:** 46m  

#### What I Did

added a 3.5mm headphone jack for the amp output and dual rca outputs for line out. worked through r/printedcircuitboard tips to clean up the schematic a little like removing gnd labels, removing the vcc_5v net, etc.

#### Media

![Log Media](https://hc-cdn.hel1.your-objectstorage.com/s/v3/6d37c43431c010f36776f9411cc04e1886f61e9a_tmp8rrs9hno.png)

---

### Cleaning up the schematic part II - August 15, 2025 <a id="cleaning-up-the-schematic-part-ii-2025-08-15"></a>

**Time Spent:** 15m  

#### What I Did

added a load switch so USB VBUS isnt more than 10uF naked. replaced VCC_3V3 nets with VCC power flags.

#### Media

![Log Media](https://hc-cdn.hel1.your-objectstorage.com/s/v3/d769d967eec84a45d4f106f6bad6c73448ac69b6_tmp9ez616hi.png)

---

### DTR and RTS pins on UART - August 16, 2025 <a id="dtr-and-rts-pins-on-uart-2025-08-16"></a>

**Time Spent:** 30m  

#### What I Did

had to research a little but apparently if i want auto-bootloader i have to configure a few more pins on the USB-to-UART chip. I've done this now! I also added a lot of breakout pins so I can debug if the pcb turns out bad

#### Media

![Log Media](https://hc-cdn.hel1.your-objectstorage.com/s/v3/49cf757b0605e2577f9e92aa2f30497f2996d316_tmp8w7rc5kc.png)

---

### Start PCB - August 16, 2025 <a id="start-pcb-2025-08-16"></a>

**Time Spent:** 45m  

#### What I Did

started laying things out on the board! Also played around with making a ground plane and autorouter

#### Media

![Log Media](https://hc-cdn.hel1.your-objectstorage.com/s/v3/03db772591ce20fc3b56325c063adcfda42d6171_tmps5id7bqy.png)

---

### pcb: lay out components - August 16, 2025 <a id="pcb:-lay-out-components-2025-08-16"></a>

**Time Spent:** 30m  

#### What I Did

finished laying out components how i think they'll go best, though obviously it'll be annoying...

#### Next Steps

start routing

#### Media

![Log Media](https://hc-cdn.hel1.your-objectstorage.com/s/v3/2931e42dc3f5d7bb9b8dec88331046ba5107fffb_tmp5uzqr75h.png)

---

### routing: part I - August 16, 2025 <a id="routing:-part-i-2025-08-16"></a>

**Time Spent:** 30m  

#### What I Did

okay so i struggled for a WHILE with how this puzzle is supposed to go together. I thought i'd try autorouter and it skipped the hard lines 😭 dude thats what i wanted to use autorouter for 😭 i guess ill be trying this myself. gonna try moving things around and see if that works a little for me to manually route

#### Media

![Log Media](https://hc-cdn.hel1.your-objectstorage.com/s/v3/28481993943d8d70f1fc3be1b7885cccb0ead173_tmp741g0pkv.png)

---

### routing part II - August 16, 2025 <a id="routing-part-ii-2025-08-16"></a>

**Time Spent:** 1h 0m  

#### What I Did

finished routing!! this is far from the final version: im going to go back and see if I've made any mistakes/anything should be different. Ive remembered to keep caps near the ICs they're meant for, route regulator output directly to high energy chips instead of through resistor pads and caps, and have a ground plane on the second level. Im gonna go through now and look for mistakes then post it for review on eevblog or something

#### Media

![Log Media](https://hc-cdn.hel1.your-objectstorage.com/s/v3/7c18eef9772515af6300c5dd2155d8ba2720494b_tmpxyrdqsr_.png)

---

### silkscreen, cleaning things up - August 17, 2025 <a id="silkscreen,-cleaning-things-up-2025-08-17"></a>

**Time Spent:** 1h 0m  

#### What I Did

moved some things around a little on the pcb, added some notes on the silkscreen, more breakout pins

#### Media

![Log Media](https://hc-cdn.hel1.your-objectstorage.com/s/v3/8cdb64ab8076f49111af39d0ecbaca7e2adaf3fb_tmpm41vfiio.png)

---

### re-placing all parts - August 18, 2025 <a id="re-placing-all-parts-2025-08-18"></a>

**Time Spent:** 1h 30m  

#### What I Did

okay apparently 
a. I can't use autorouter because it won't differentiate mixed signal traces
b. I need to physically separate analog and digital components
so, I've undone all the traces and replaced each component. 
some pros of this:
1. the board is now shorter on its length
2. caps and resistors are even closer to their ICs

#### Next Steps

route by hand

#### Media

![Log Media](https://hc-cdn.hel1.your-objectstorage.com/s/v3/0f063ee3bd97c2fae2155c9a65fd802ccfd24149_tmpq8ar3ec0.png)

---

### finished routing! - August 18, 2025 <a id="finished-routing!-2025-08-18"></a>

**Time Spent:** 1h 0m  

#### What I Did

finished routing! some parts were a little annoying but good placement = faster routing so it wasn't bad doing it by hand either.

#### Next Steps

silkscreen notes, start cadding case

#### Media

![Log Media](https://hc-cdn.hel1.your-objectstorage.com/s/v3/717de67bdaa48e1430cdcc53d464bfe6bc49130b_tmplagfomke.png)

---

### case - August 19, 2025 <a id="case-2025-08-19"></a>

**Time Spent:** 1h 0m  

#### What I Did

made the case in an hour! wasn't too hard, I just needed to:
1. make the shell
2. cut out holes for all the ports and the antenna
3. add ribs for snap fit
4. filets

#### Media

![Log Media](https://hc-cdn.hel1.your-objectstorage.com/s/v3/fda18203e4fa19dcb4d3f6be5dab7e549fa2ae78_tmps4jd9t2y.png)

---

### cleanup - August 23, 2025 <a id="cleanup-2025-08-23"></a>

**Time Spent:** 1h 0m  

#### What I Did

cleaned up the pcb a little and fixed the case. I needed to move some connectors and components to add more spacing between them, remove acute angle connections, and adjust traces so they leave the pad near the center.

#### Media

![Log Media](https://hc-cdn.hel1.your-objectstorage.com/s/v3/bf10fd28daa60b6c9f121083e1eef021a42fa2cc_tmpvvyawgaf.png)

---



*Exported from Grounded Tracker on 2025-08-23 18:39:41*
