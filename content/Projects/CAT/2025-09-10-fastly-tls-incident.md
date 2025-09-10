### Artifacts:
[recording of event](https://app.bluedothq.com/preview/68bf1cc101cb4bf7126f060d)

### Part of affected domains:
https://gamacasino477.com

https://gamacasino437.com/

https://194.landing-to-arkada.com/

https://daddycasino80.com/


### How it was seen from user side:
https://grid-cyberlane.com/sd29a9426


## Timeline:

**CET**

- **19:51:** An alert was received regarding Gama mirrors (start of the incident).
    
- **19:56:** A request was sent to this chat.
    
- **20:02:** The team was engaged. In the Fastly admin panel, we noticed that certificates for over 200 hosts, not just for Gama, had disappeared. This included landing pages for all products. We began manually applying the certificates.
    
- **20:18:** All certificates were automatically restored (the reason is currently unknown). We began checking availability: Arkada and Motor were confirmed as operational. Gama's availability was still in question, with partial access.
    
- **20:41:** We received confirmation from QA that Daddy and Kent were also working.
    
- **20:42:** Gama mirrors became operational, and monitoring stabilized.
    
- **20:53:** We received confirmation from QA that Gama was also working again (end of the incident).
    

---

**Summary of Impact:**

- **Kent / Arkada / Motor / Daddy:** 
	- Partial unavailability of mirrors/landing pages from 20:00 to 20:20-20:30. Total downtime was about 30 minutes.
    
- **Gama:**
    - Complete unavailability of mirrors/landing pages from 19:51 to 20:18 (about 30 minutes).
    - Partial unavailability of Gama mirrors from 20:18 to 20:42 (24 minutes).

**Total downtime for Gama** was 1 hour (19:51 - 20:53).
