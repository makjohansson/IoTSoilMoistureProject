## Table of contents
* [Background](#background)
* [Project process](#project-process)
* [Issues](#issues)
* [Hardware](#hardware)
* [Software](#software)
* [Result](#result)
* [Figures](#figures)

## Background
Last autumn Kalmar Kommun and Fredrik Ahlgren held a presentation over different IoT projects. One project presented was a project that would with the help of IoT solve some problems Kalmar Kommun has when it comes to the watering of plants and gardens around the county. Now they have to drive large distances to check if water is needed in the plants. If this problem is solved it would make Kalmar Kommun able to use their stab more effectively and also the cost for the fuel would go down because of the removal of unnecessary trips. Lnu and Kalmar Energi provide communication over LoRa in Kalmar. Kalmar Kommun has the ambition to use the LoRa network in their work to become a smarter city. Therefore the LoRa technology was used in this project.
	
## Project process
The project started with the research of different types of moisture sensors. Many articles, experiments, and blogposts were read. This research resulted in the conclusion that the Capacitive sensor is superior to the resistive sensor. The resistive sensor has exposed metal parts that corrode after just a short time period. This creates issues with values received from the sensors and a user of the sensor would have to recalibrate it after just a few days. This was experienced in this project when using the resistive sensors during the first week of the project. The resistive sensors were used during the learning phase with LoRaWAN. And after two days the sensors started to corrode and the values received from the sensors were not reliable. The lowest value received in the calibration was now much higher (calibration is explained later in this section). The main reason the resistive sensors were used for a couple of days while implementing and testing LoRaWAN was that the capacitive sensor purchased for this project turned out to have a defect. The defect made the sensor unusable, the value received from the sensor did not differ if the sensor was completely in water or dry air. Comments on e-commerce sites selling the sensor confirmed that the defect was well known. A decision was made to look for other capacitive sensors. A sensor with good reviews from Adafruit was found and that sensor was ordered. While waiting for the sensor to arrive work proceeded with the resistive sensors and the server development started. When the sensor arrived it was calibrated. The calibration process starts with receiving a value from the sensor when the sensor is in 100% water and one value when the sensor is in dry air. The values are set to the sensor as DRY_VALUE and WET_VALUE. A formula covering the values to percent is implemented and the value from the sensor is received as a value between 0 - 100 % moisture. After a few days with the Adafruit sensor and good results from tests, a decision was made to add one more sensor to the project. This time a sensor from DFRobot. The value from the sensor is read with an analog pin which resulted in a quick implementation without any issues. With two working sensors test began to monitor the behavior of the sensor in soils of variant moisture level (see figure 1). Measurements was made with a moisture meter for soil and compared with the value received from the sensors and the two values correlated. For testing purposes a sensor without Lora connection was built and installed in Kalmar kommuns greenhouse. The staff in the green house could use this sensor to test the sensors behavior. The experience the staff possesses in watering plant could in this test be used to test the sensor (see figure 2). Parallel to the sensor testing the development of the server-side on Openstack proceeded. The DFRobot sensor was bought with water protection. The Adafruit sensor was made waterproof by using epoxy glue on the exposed electrical parts (see figure 3). The hardware parts is placed in waterproof electrical boxes. Now the sensors are ready for real environmental testing. After the sensors where placed in the greenhouse, values from the sensors was received ten times in twenty-four hours. Parallel to the monitoring of the sensors the development of an Android application used to send and receive values from the sensor began.

## Issues
* **Adafruit sensor:** The sensor is using the i2c bus and circuitPython. The microcontroller from Pycom uses MicroPython. Before the sensor could be used a library was implemented for MicroPythons i2c bus.

* **NVRAM function:** When implementation of the deep sleep function on the Pycom LoPy4 a criterion was that the LoRa device must remember that it has joined a network when waking up. This saves battery by not having to go through the join sequence every time the device wake up. The LoPy4 device has support for this. When implementing this functionality an issue was that the NVRAM function did not work with the deep sleep function. After some time debugging the issue was resolved by downgrading the firmware for the LoPy4 one release version.

* **SSL connection:** The implementation of the TCP server is in python and during the first stages of development, a python script was used as a client to act as the Android application. The implementation of SSL Python-server to Python-client was simple. The problem emerged when implementing the SSL connection using Java. Java is the language used to build the application. Implementing SSL in Java vs Python turned out to be very different. It was a bit harder to figure out the Java approach to SSL. Java uses Keystore and TrustManager to make certification validations and decryptions. It took some time but after extensive research in OpenSSL, keytool, SSL, and certificate formats the issue was solved.

* **LoRa connection:** In the greenhouse, the devices could not connect to the LoRa network provided by Lnu. This issue was solved by configuring a LoRa router and placed that router in the greenhouse.

* **Waterproof box:**  A design of a waterproof box was created using Fusion360 and the plan was to print that design using the 3D printer Lnu provides. The issue emerged when it was time to print that design. The printing time for the box was 1 day and 10 hours. The printing process has to be supervised and due to the printing time that was not an option. The issue was solved by buying waterproof boxes.

## Hardware
- **Pycom**
    - LoPy 4
    - Expansion board 3.1
- **Sensor**
    - DFRobot Gravity Capacitive Soil Moisture Sensor
    - Adafruit STEMMA Soil Sensor
- **Application**
    - Android device

## Software
- **Application**
    - Android application with a ListView and a DetailedView
- **Server on Lnu Openstack**
    - Python script running TCP connection with application
    - Python script running TTN api collecting and sending data to sensors

## Result
**Video demonstration** on [YouTube](https://youtu.be/RBFp-bJJFYI) 
Music by Martin "Bubba" Johansson 
Camera and editing by Therese Johansson

**The state of this project at the end the course:**
* Two working sensors place in a greenhouse sending moisture values 2,4,6,8 or 10 times in twenty-four hours, depending on the end-users preferences (see figure 4 and 5)

## Figures

![alt text](https://github.com/makjohansson/IoTSoilMoistureProject/blob/master/img/IMG_1547.jpeg?raw=true) 
<center>**Figure 4:** Adafruit sensor in the greenhouse</center>

![alt text](https://github.com/makjohansson/IoTSoilMoistureProject/blob/master/img/IMG_1546.jpeg?raw=true)
**Figure 5:** DFRobot sensor in the greenhouse