# Monitoring-System

BMP180 Pin configuration :
VCC: Connected to +5V
GND: Connected to GND
SCL: Serial Clock pin
SDA: Serial Data pin (I2C interface)
SCL & SDA are used to communicate with the ESP32 module. The data is sent to the ESP32 or received from the ESP32 using these two pins.
To communicate data to and fro we use the I2C communication protocol.

I2C communication Protocol
I2C communication is the short form for inter-integrated circuits. Using just two common wires, I2C allows data to be transferred between a central processor (ESP32) and several ICs on one circuit board.

Open Arduino IDE, Go to Tools, and then Manage Libraries
Type BMP085 and then click on Install.
After Installing sensor library a window will appear

Note:When running any Arduino program, if a library error appears, follow the same instructions to install libraries.
Install BMP180
Install MQTT library

Step -1:Gather the material from the IoT kit:
● 1 x ESP32
● 1 x USB Cable
● 1 x Breadboard
● 4 x Jumper wires
● 1 x BMP180

Step -2: Let’s do connections:
BMP180 Pins Wiring Connections
VCC Connect with 3V3 PIN of the ESP32
GND Connect with GND of the ESP32
SCL Connect with GPIO PIN 22
SDA Connect with GPIO PIN 21

Step-3 Let’s write a code:
Define the libraries
● Wire.h library is used to communicate with I2C devices.
● Adafruit_BMP085.h library is used for pressure sensors.
● Create object bmp for Adafruit_BMP085

Initialize the setup()
● Serial. begin(9600) is used for data exchange speed. This tells the Arduino to get ready to exchange messages with the Serial Monitor at a data rate of 9600 bits per second. That's 9600 binary ones or zeros per second and is commonly called a baud rate.
● bmp.begin() is used to begin the process.
● Serial.printIn is used to print data. Print (“Could not found”, if it fails to begin the process)

To execute the main process write the void loop()
● Serial.print is used to print data
● readTemperature() will read the temperature value.
● readPressure() will read the pressure value.
● Set the delay of 500 ms

Output:
Compile and upload the program to ESP32 board using Arduino IDE
● Verify the program on clicking Tick option
● Upload the program on clicking arrow option

Note: If the port is not selected, insert the USB cable in Computer’s port and select the port

● Go to Tools and select Serial Monitor
○ Pressure values will be displayed in Pascal. Pascal is a unit used to measure Pressure

We want to send data on a cloud server, for that we need to use an online server and to access an online server we need to use the platform Adafruit.

Adafruit will act as a broker between your device and server. Basically Adafruit is a neutral party that your Things can connect to send and receive messages.
Let’s set up an online server - https://accounts.adafruit.com/users/sign_in

Click on Sign Up
● Add your Sign-in details
● Click on Create Account
● Click on IO
● Go to Dashboards
● Click on New Dashboard
● Write the Name (Environment Monitor System)
● Write Description if needed
Click on Create New Block
Select the Gauge: Gauges are visual blocks to represent sensor values

Connect a Feed
Feed - this is basically a set of data that you can read or write from like a sequential file. We can add data and we can receive the latest added data using feeds.

Select the Feed which you have created just and then click on Next Step

Select the default values and click on Create Block
In default, a thin type gauge will be used with min value 0 and max value 100. We can change the max value as per our wish.
Repeat the above steps to create one more Gauge for Room Pressure

Repeat the same steps to create one more Gauge for Room Temperature
After creating two gauges one for Room Pressure and one for Room Temperature below the window will appear.
The gauges are now added to the dashboard

Next up, make another block, this time an on-off toggle switch for LED’s
Now select the two Toggle buttons for Room AC and Room Light.
Set the default values for “on” and “off” texts

Now after selecting Toggle for AC & Light, a window will appear. Now, drag the Feeds to set the positions properly. Click on Save Layout.

Now we have prepared our cloud server on Adafruit, our next step is to integrate the BMP180 sensor and LEDs.
After integration of BMP180 sensor and LEDs will send real time data on Adafruit Dashboard.



Step -1:Gather the material from the IoT kit:
● 1 x ESP32
● 1 x USB Cable
● 1 x Breadboard
● 4 x Jumper wires
● 1 x BMP180
● 2 x LED
● 2 x Resistors
Mount the components

Step -2: Let’s do connections:
BMP180 Pins Wiring Connections
VCC Connect with 3V3 PIN of the ESP32
GND Connect with GND of the ESP32
SCL Connect with GPIO PIN 22
SDA Connect with GPIO PIN 21

LED Connections: Connect positive leg of LED with the resistor and negative with GND(0V).
Another end of the resistor with GPIO pin 18.
Do the same connection for the second LED, but this time connects with GPIO pin 19

Step -3: Let’s write a program
1. Upload Libraries
2. Connect with ESP32 with the WiFi. For that, we need to use SSID(Wi-Fi credentials i.e WiFi name and WiFi Password)
3. Adafruit Setup and Authentication- Set the Adafruiit AIO_SERVER , AIO_SERVERPORT, AIO_USERNAME, AIO_KEY
   To get AIO_USERNAME & AIO_KEY go to adafruit, Click on My Key
   After clicking on My Key, a window will appear.

   Key - This is a long, unique identifier that is used to authenticate devices using an Adafruit account.
   Note down IO_USERNAME & IO_KEY, paste the same in the program where AIO_USERNAME & AIO_KEY is mentioned.

   Publish & Subscribe
   Publish - push data from device to server for example Sensor Data
   Subscribe- push data from server to device for example LED Control

4. Setup the MQTT client class by passing is Adafruit server set up details like AIO_SERVER, AIO_SERVERPORT, AIO_USERNAME, AIO_KEY
5. BMP180 which is used to sense temperature and pressure will use Adafruit_MQTT_Publish publish data, and to control LED from the server we use Adafruit_MQTT_Subscribe AIO_USERNAME/feeds/temperature is feed name
6. MQTT_ connect will establish a connection og ESp32 with cloud server

7. Define GPIO pins
● Define LED’s pin along with data type int
● Define Variable p with datatype float
● Define String variable to store value
● Define bmp object for Adafruit_BMP085

8. Initialize the setup()
● Serial. begin(9600) is used for data exchange speed. speed parameters. This tells the Arduino to get ready to exchange messages with the Serial Monitor at a data rate of 9600 bits per second. That's 9600 binary ones or zeros per second and is commonly called a baud rate.
● Set a delay of 10 ms
● PinMode() configures the specified pin to behave either as an input or an output. As we want to act as output we are writing OUTPUT here. Set the pinMode for both Led1, Led2
● digitalWrite() function helps to change the state of LED from HIGH to LOW or vice versa

9. Serial. println is used to print the statement

10. Setup MQTT subscriptions for led1, led2 i.e. sw1,sw2. sw1 and sw2 are the feed names that we entered at the very first step of naming the feed.
● bmp. begin() is used to start the process.
● Print (“Could not found”, if the sensor fail to start the process

11. 1The unsigned uint32_t data type works on 32-bit numbers.
To execute the main process write the void loop()
● The readPressure() function will read the pressure around us
● The readTemperature () function will read the pressure around us
● Store the sensor’s pressure value into variable p
● Set a delay of 100 ms
● MQTT_connect function instructs the library to start connecting to the server. This will ensure the connection to the MQTT server is alive.

12. We know to send data from server to end devices(LEDs) we need to use the Subscribe function
13. Now, it's time to publish the BMP180 data, We need to publish Pressure and Temperature, The publication is much easier than subscribing, we need to just call the publish function of the feed object.

14. Output:
Compile and upload the program to ESP32 board using Arduino IDE
● Verify the program by clicking the Tick option
● Upload the program by clicking the arrow option

Note: If the port is not selected, insert the USB cable in Computer’s port and select the port
● Go to Tools and select Serial Monitor

Error Message: If an error message comes like no such file or directory then use the below method to resolve this error
Go to Tools
● Click on Manage Libraries
● Write the component name which needs to install
● Click on Install

Go to Tools and select Serial Monitor
● See the Pressure and Temperature value
● Open your Adafruit server and check the live values of Pressure and Temperature. Click the Toggle buttons to turn ON and OFF your LED’s
