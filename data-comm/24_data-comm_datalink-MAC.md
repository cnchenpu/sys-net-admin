Please refer to textbook [chapter 12](https://github.com/cnchenpu/data-comm/blob/master/ppt/Ch12-Forouzan.ppt). <br>

# Media Access Control
- Lower layer of __Data Link Layer__, provide addressing and shared media access control - contorl who can access and how to access the media.
- 1. Random Access (Contension)
- 2. Controlled Access
- 3. Channel Division

## Random Access
- No pirority, no schedule, transmission is selected __randomly__.
- No central supervisor, transmission could have __collision__.
- To solve the collision, every station have to determine:
  - when can access media?
  - what to do if media is busy?
  - transmission success or failure?
  - what to do if collision happened?
  
### ALOHA
- __Multiple Access__ - send as need
- Whenever a terminal has data, it transmits. Sender finds out whether transmission was successful or experienced a collision by listening to the broadcast from the destination station. Sender retransmits after some random time if there is a collision.

![](fig/ALOHA.png)

#### Pure ALOHA protocol
```
1. Transmit a frame.
2. IF ACK arrived
3.    done.
4. ELSE
5.    Wait a random time and GOTO 1
```

![](fig/ALOHA-protocol.png)

#### Why need to wait for 2 times of frame transmission time
Vulnerable time is 2 times of frame transmission time ___T<sub>fr</sub>___ <br>

![](fig/ALOHA-Tfr.png)

> EX: <br>
> A pure ALOHA network transmits 200-bit frames on a shared channel of 200 kbps. <br>
> What is the requirement to make this frame collision-free? <br>
> <br>
> Average frame transmission time T<sub>fr</sub> is 200 bits/200 kbps or 1 ms. The vulnerable time is  2 × 1 ms = 2 ms. <br>
> This means no station should send later than 1 ms before this station starts transmission and no station should start sending during the one 1-ms period that this station is sending.

### Slotted ALOHA
- Divide time to different frame transmission time slots.
- Stations can only send at the start time of the slot.
- Collision still could happen but, reduce the vulnerable time a half.

![](fig/ALOHA-slot.png) 

![](fig/ALOHA-slot-Tfr.png)

## Carrire Sense Multiple Access (CSMA)
- Sense before transmission.
- Reduce collision, reduce retransmit, increase performance.

- Vulnerable time is propagation time <br>
![](fig/CSMA-Tp.png)

### How to sense the media is busy or available.
- __Persistence Methods__
  - 1-persistent 
    - keep sense until medium is available 
    - transmit if medium is available
    - collision rate is highest
  - non-persistent
    - if medium not available wait for a random time before next sense
  - p-persistnet
    - keep sence until medium is available
    - if medium is available, transmit in probability p
    - wait for next time slot to sence the medium in proability 1 - p
    
## Carrire Sense Multiple Access with Collision Detection (CSMA/CD)

> EX:
> 1. A sent frame at t<sub>1</sub> from A to D
> 2. C sent frame at t<sub>2</sub> (not detect A's first bit of frame yet) in both sides
> 3. Collision happened after t<sub>2</sub>
> 4. C detect collision at t<sub>3</sub>
> 5. A detect collision at t<sub>4</sub>
> 
> - A's transmission time is t<sub>4</sub> - t<sub>1</sub>
> - C's transmission time is t<sub>3</sub> - t<sub>2</sub>

![](fig/CSMA-CD-ex.png)

### The minimum frame size in CSMA/CD
- Sender have to detect the collision before transmit its last bit in the frame.
- Since after transmitted the complete frame, station will not keep the frame data and will not sense the medium.
- So the frame transmission time ___T<sub>fr</sub>___ should be twice of the maximum propagation time ___T<sub>p</sub>___ .
  - EX: The frame needs ___T<sub>p</sub>___ to arrive the receiver, so does the collision signal, so station should keep transmission in ___2T<sub>p</sub>___ .

> EX: <br>
> In the 10Mbps CSMA/CD Ethernet the most transmission time is 25.6us, what is the minimum frame size? <br>
> <br>
> The frame transmission time ___T<sub>fr</sub>_ = 2 x _T<sub>p</sub>_ = 51.2us__ <br>
> That means the station need 51.2us to detect collision. <br>
> The minimum frame size = 10Mbps x 51.2us = 512 bits = 64 byte.

### CSMA/CD protocol
- similar to ALOHA
- Use __Persistence Methods__ to sense the medium
- Transmit and detect collision at the same time
  - ALOHA send a complete frame then wait the ACK
- Send __jamming signal__ to notify not find collision yet

![](fig/CSMA-CD.png)

## Carrire Sense Multiple Access with Collision Avoidance (CSMA/CA)
- Wireless LAN is hard to detect collision
- Collision Avoidance strategy:
  - Interframe Space (IFS)
    - Even find the medium is available, still wait for an __interframe space__ time
    - High priority station has shorter IFS
  - Contention Window
    - choose a random number of slot of wait time
    - every station has different slot of wait time
  
![](fig/CSMA-CA-timing.png)

1. keep sense until medium is available
2. wait for a inter-frame space (IFS) time after find medium is available
3. sender send a control frame - __RTS (request to send)__
4. after receive RTS and wait for a short inter-frame space (IFS) time, destition station send a control frame - __CTS (clear to send)__
5. wait for a short inter-frame space (IFS) time sender send the data
6. wait for a short inter-frame space (IFS) time, receiver confirm (reply ACK) the sender

![](fig/CSMA-CA.png)

## Controlles Access
Negotiation to determine which station can transmit.
- Reservation
  - each station have to reserve the time slot <br>
  ![](fig/reservation.png)
- Polling
  - every data exchange have to pass through the primary station <br>
  ![](fig/polling.png)
- Token Passing
  - token ring <br>
  ![](fig/token-passing.png)

## Channel Division
Can also refers to [chapter 6](https://github.com/cnchenpu/data-comm/blob/master/15_data-comm_multiplexing.md)
- FDMA
  - In FDMA, the available bandwidth of the common channel is divided into bands that are separated by guard bands.
- TDMA
  - In TDMA, the bandwidth is just one channel that is timeshared between different stations.
- CDMA
  - In CDMA, one channel carries all transmissions simultaneously.

