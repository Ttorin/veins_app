# MyVeinsApp for Space-Air-Ground Vehicular Network Framework

## Overview
### 1, Background of the Framework
Our Space-Air-Ground (SAG) vehicular network framework focuses on mobile edge computing among vehicles, road side units (RSU) and some aerial communication platforms like unmanned aerial vehicles (UAV). The vehicle in the framework offloads its tasks to other nodes with computing capability. Therefore, the total computational delay is reduced and the computing resources are highly utilized. 

Autonomous Vehicular Edge (AVE) is a framework for edge computing on the road to increase the computational capabilities of vehicles. \[1\] The author provides a solution with ACO (Ant Colony Optimization) algorithm for the task offloading problem in dynamic environment among decentralized vehicles, which can allocation the computing resources more efficiently. Considering the reasonableness of the framework for practical vehicular network, we mainly realize our framework based on it with some modifications. 

One important difference is the assumption we make that one vehicle must be either requester (generate jobs and request computing resources) or processor (process jobs) , unlike that of AVE framework where every vehicle is both requester and processor. We suppose that only specific vehicles and platforms can provide computing resources in the near future. This difference causes a few changes of the stage, which will be explained in the following parts. 

### 2, About the Softwares
The framework is based on Veins, which is simulator for vehicular network. It combines SUMO, a traffic simulation platform, and OMNeT++, an IDE for network simulation and realizes a great amont of communication elements such as IEEE 802.11p, WAVE and two-ray interference channel, which makes it convenient to build a vehicular network framework and analyze the simulation results. \[2\]

The framework works on both Linux and Windows systems and it is mainly built and tested on ubuntu 16.04 LTS. SUMO 0.30.0, OMNeT++ 5.1.1, Veins 4.6 are used for simulation. For details about installation of Veins, please refer to [Veins tutorial][1]. You can also get enough information of SUMO and OMNeT++ there. 

[1]:http://veins.car2x.org/tutorial/

## Data Structures
### 1. Messages
Different from the cMessage which general OMNeT++ application used, Veins encapsulates specific message classes according to Dedicated Short-Range Communications (DSRC) Standards. \[3\] There are 3 kinds of messages it uses, which are WaveShortMessage(WSM), WaveServiceAdvertisement(WSA) and BasicSafetyMessage (BSM). WSM is mainly used by us to send messages required by our framework. 

Since many packets are directly sent from source to destination without routing in the vehicular environment, short messages are preferred to avoid great overhead. Therefore, WAVE Short Message Protocal (WSMP) is defined for more efficient 1-hop transmission and WSM is the packet using WSMP. Its head length ranges from 5 bytes to 20 bytes, which is far smaller than that of original IPV6 packets. WSA is the WSM which includes some DSRC service information and BSM is the WSM which includes some safety information like the current position and speed of the sender. 

To send a WSM, we assert a WaveShortMessage*, set its data (a string of at most 4KB) and schedule its transmission. When receiving a WSM (WSA, BSM), the program calls onWSM() (onWSA(), onBSM()) to process the packet. To identify various messages we use in different cases, we set several headers as follows: T (traffic message of original code), B (my beacon in the framework), Q (EREQ at discovery), P (EREP at discovery), D (data content at transmission), J (job brief of data).     

### 2. Jobs
struct *job*, queue\<job\> *job_queue*vector\<job\> *job_vector*

### 3. NAI (Neighbor Availability Index) Table
NAI entry, NAI table

### 4. Work
map\<int, job\> *work_info*

## Stages
Event driven framework
### 1. Beaconing
#### 1) Phase in processor: send beacons

#### 2) Phase in requester: receive beacons

### 2. Job Caching
* Phase in requester: generate jobs

### 3. Discovery
#### 1) Phase 0 in requester: request information

#### 2) Phase in processor: make response

#### 3) Phase 1 in requester: collect information

### 4. Scehduling

### 5. Data Transmission
#### 1) Phase 0 in requester: send job brief & data

#### 2) Phase in processor: receive & process job

#### 3) Phase 1 in requester: get result

## References
\[1\]  J. Feng, Z. Liu, C. Wu and Y. Ji, "[AVE: Autonomous Vehicular Edge Computing Framework with ACO-Based Scheduling][2]," in IEEE Transactions on Vehicular Technology, vol. 66, no. 12, pp. 10660-10675, Dec. 2017.

[2]: http://ieeexplore.ieee.org/stamp/stamp.jsp?tp=&arnumber=7946184&isnumber=8207705

\[2\] Christoph Sommer, Reinhard German and Falko Dressler, "[Bidirectionally Coupled Network and Road Traffic Simulation for Improved IVC Analysis][3]," IEEE Transactions on Mobile Computing, vol. 10 (1), pp. 3-15, January 2011.

[3]: http://ieeexplore.ieee.org/stamp/stamp.jsp?tp=&arnumber=5510240&isnumber=5640589

\[3\] J. B. Kenney, "[Dedicated Short-Range Communications (DSRC) Standards in the United States][4]," in Proceedings of the IEEE, vol. 99, no. 7, pp. 1162-1182, July 2011.

[4]: http://ieeexplore.ieee.org/stamp/stamp.jsp?tp=&arnumber=5888501&isnumber=5888494

