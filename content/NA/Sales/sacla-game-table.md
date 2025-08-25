# Goal: provide steps to build Sacla live tables


# Steps
- check on offline part
- check on legals part
- build online part
- launch

## Offline part
Studio:
1. **Physical location** (based on the number of tables, the need for future expansion, VIP tables, climate, geographical location, etc.)
2. **Personnel** (control over the selection of location, control over the implementation of construction, quality control of equipment, selection of line and management personnel, staff training) 
3. **Project design**, approval, and control of the project's implementation.
4. **Selection**, approval, and ordering of equipment, taking into account depreciation, development plans, and spare parts.
5. **Project handover/completion.**
    
The average cost of one table is **$25,000 — $30,000** (this is an approximate cost that can be either reduced by lowering the quality and quantity of the equipment or increased). These numbers include server equipment (it means that from 5th table the numbers also will go down)

For example, when using top-of-the-line cameras for a table, the cost of equipping one table can be **$50,000 — $80,000**.

The final cost can only be provided after clarifying all the requirements for the equipment (quality of cameras, lenses, microphones, streaming, server room, etc.).

## Check on legals part
Sakla is illegal on the Philippines. There is no local card-club license, that allows to play this game.

Also, Dealers need to be [certified by local authority](https://www.pagcor.ph/regulatory/pdf/Casino/Implementing-Rules-and-Guidelines-for-the-Video-Streaming-Player-Liaison-Program.pdf), 
>Certificate of Training in casino table games issued by any PAGCOR accredited training facility.

But they have only
1. Baccarat based game 
2. Blackjack 
3. Pontoon 
4. Roulette
5. Craps 
6. Pai-gow Poker 
7. Texas Hold'em Poker

No Sakla certification available from PAGCOR, it means that tables should be placed outside the Philippines.

## Build online part (software)
The software consists of three parts
- server part
- local part
- admin part


### Server part
It's actually a game api, with standard seamless-wallet api, to provide game to different online providers. Also, it handles all game session, web-ui and everything accessible by player. The main quirk here is to make time-attack protection.
#### ReStream appliance
We collect video stream from local appliances, and republish it as HLS stream to users.

### Local part
Inside the local venue we need to capture stream and cards data. Then data should be passed to Server part and ReStream appliance.

### Launch
On a launch step we need to align online casinos\aggregation connection within people available in place


## Totals
Need more qualification on offline\legals part. From first glance, it looks like there are some complications in those steps. Depending on request, we can make a physical object project (if you're planning to build a new venue). Also, when got a decision on a place and legal part — we can move further on software part. In common, the project timing starts from half a year. Exact time will depend on further clarification.
