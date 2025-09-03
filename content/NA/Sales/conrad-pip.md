# Goal: build an appliance to embed pip window inside slot machine

### Input
Conrad have lots of:
IGT 660 3.0 AVP G20
Ceronix VGA/DVI
USB 3M Touch
![conrad-device-6.jpeg](./attachments/conrad-device-6.jpeg)
![conrad-device-6.jpeg](./attachments/conrad-device-5.jpeg)
![conrad-device-6.jpeg](./attachments/conrad-device-4.jpeg)
![conrad-device-6.jpeg](./attachments/conrad-device-3.jpeg)
![conrad-device-6.jpeg](./attachments/conrad-device-2.jpeg)
![conrad-device-6.jpeg](./attachments/conrad-device-1.jpeg)
We want to build a bonus system, similar to https://www.ganlot.com/news/press-releases/ganlot-launches-4k-gaming-mixer-of-outputs/ and https://www.igt.com/products-and-services/gaming/systems/marketing/service-window

It should be a picture in picture window, in which people should be able to see internal bonus system and other data from internal game management system.

Player should be able to interact with this frame using touch screen.

No integration with buttons needed.

No integration with cash-in\cash-out needed.

No integration with counters needed.

### What we have
We have found similar old device, that should have the same version of touchscreen.

![[conrad-device-alien.jpeg]]

## Milestones
### connect "Alien" to existing system
At first we need to prove that we can work with touch screen. We should connect alien to our appliance, to show that we control its touch screen.
### test touch on Conrads device
After that we need to test that it will work with actual devices. We sell one RLS device, which Conrad will setup into one of his devices. We will connect to RLS to prove that it works (we can connect it to RLS stage, to play with fake money)

if everything works, we can move forward. On this step we need to agree on pricing. Next steps done in parallel.
### create software appliance
We should create a software, that will proxy video stream and add actionable button. The button should be draggable. Tapping the button should open onscreen window. This window should open an iframe from external server.

### create hardware design
We should create a specification and design for device (like gerber files etc).

### produce devices
We produce devices and send them to USA.

