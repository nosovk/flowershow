# Project: PiP-in-Slot Machine Appliance
## Goal: Build a hardware and software appliance to embed a Picture-in-Picture (PiP) window inside a slot machine's display.

### Project Context & Input
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
The desired final product should be a bonus system similar to those seen in the following examples:
- https://www.ganlot.com/news/press-releases/ganlot-launches-4k-gaming-mixer-of-outputs/ 
- https://www.igt.com/products-and-services/gaming/systems/marketing/service-window

The PiP window should display the internal bonus system and other data from the internal game management system. Players must be able to interact with this on-screen window using the machine's touchscreen.

Player should be able to interact with this frame using touch screen.

#### Not to implement:
- Integration with physical buttons.
- Integration with cash-in/cash-out mechanisms.
- Integration with internal counters.

### What we have
We have found an old device "Alien", that is believed to have the same version of the touchscreen technology. This will be used for initial testing.

![[conrad-device-alien.jpeg]]

## Milestones
### Connect "Alien" to existing system
The first step is to prove that we can successfully work with the touchscreen. We will connect the "Alien" device to our appliance to demonstrate that we can control its touch screen.
### Test touch on Conrads device
Once the "Alien" device has been verified, we will test the functionality on the actual target devices. We will sell one RLS device, which Conrad will set up in one of his machines in America. We will then connect to the RLS stage to prove that the system works (we can use fake money for testing purposes).

If this step is successful, we can move forward. Upon completion of this milestone, we need to agree on pricing. The following steps will be performed in parallel.
### Create software appliance
We will create the software required to:
- Proxy the video stream.
- Add a draggable, actionable button to the display.
- Open an on-screen window when the button is tapped.
- Display an `iframe` from an external server inside this window.
### Create hardware design
We should create a specification and design for device (like gerber files etc).

### Produce devices
Upon completion of the design, we will produce the devices and ship them to the USA.

## Questions
We need to determine the total number of devices to produce. This information is important for accurate cost estimations and production planning.