# NRF24L01+ transmitting part with Arduino

The nRF24L01+ transceiver module is designed to operate in 2.4 GHz worldwide ISM frequency band and uses 
GFSK modulation for data transmission. The data transfer rate can be one of 250kbps, 1Mbps and 2Mbps.The 
nRF24L01+ transceiver module transmits and receives data on a certain frequency called Channel. Also in 
order for two or more transceiver modules to communicate with each other, they need to be on the same channel.
This channel could be any frequency in the 2.4 GHz ISM band or to be more precise, it could be between 2.400
to 2.525 GHz (2400 to 2525 MHz).

[More Information](https://lastminuteengineers.com/nrf24l01-arduino-wireless-communication)

![alt text](https://lastminuteengineers.com/wp-content/uploads/arduino/Arduino-Wiring-Fritzing-Connections-with-nRF24L01-PA-LNA-External-Antenna-Wireless-Module.png)


## Pin Connection

|Pin Name|Arduino Nano | Arduino Mega | Arduino UNO|
|---|---|---|---|
|MOSI|11|11|51|
|MISO|12|12|50|
|SCK|13|13|52|
|CE|7|8|9|
|CSN|7|8|53|
## Transmitter Code:
```
#include <SPI.h>
#include <nRF24L01.h>
#include <RF24.h>
RF24 radio(7, 8); // CE, CSN
const byte address[6] = "00001";
int data;
int potpin0 = 0;  // car LEFT-RIGHT  AO
int potpin1 = 1;  //ARM SIDEWISE A7
int val0;
int val1;
const int buttonPin_movement = 2;
int buttonState_move = 0;
void setup() {
  radio.begin();
  pinMode(buttonPin_movement, INPUT);
  radio.openWritingPipe(address);
  radio.setPALevel(RF24_PA_MIN);
  radio.stopListening();
}
void loop() {

 buttonState_move = digitalRead(buttonPin_movement);

 if (buttonState_move == HIGH)              //========================= movement 
 {
  val0 = analogRead(potpin0);            // car LEFT-RIGHT  AO
   val1 = analogRead(potpin1);            // CAR FORWARD-BACKWARD A1
   if(val0<300)
   {
   data=1;
  radio.write(&data, sizeof(data));
   }
    else 
   {
  data=2;
  radio.write(&data, sizeof(data));
   }
 
 }
 else   
 {  const char text[] = "Hello World";
  radio.write(&text, sizeof(text));
  delay(100);}
}
```




## Header Files:

```
 #include <SPI.h>
 #include <nRF24L01.h>
 #include <RF24.h>
```
*NRFL01+ is based on SPI communiation which is handled by the`SPI.h` library. For controlling the module `nRF24L01.h` & `RF24.h` libraries are used*

## Class pin declaration & Address Function:

```
 RF24 radio(7, 8); // CE, CSN
 const byte address[6] = "00001";
```
*Here, we create an `RF24` object on first line. CE pin is always an input with respect to the 24L01. It is used to control data transmission and reception when in TX and RX modes. CSN stands for chip select not. This is the enable pin for the SPI bus, and it is active low (hence the “not” in the name). You always want to keep this pin high except when you are sending the device an SPI command or getting data on the SPI bus from the chip. When this pin goes low, the 24L01 begins listening on its SPI port for data and processes it accordingly.
The second line assigns a particular address similar to the receiver so that both the tranmitter and receiver can communicate with each other*


## Setup Part:

```
 radio.begin();
 radio.openWritingPipe(address);
 radio.setPALevel(RF24_PA_MIN);
 radio.stopListening();
```
*`radio.begin()` and using the `radio.openWritingPipe()` function we set the address of the transmitter. Finally, we will use the `radio.stopListening()` function which sets module as transmitter.*



## Data assigned and trasmitted:

```
 data=1;
 radio.write(&data, sizeof(data));
```

*Here we assign an integer number to our variable `data`. We can use any integer number. One assigned, we used the `radio.write()` class to transmit the value of the variable `data`.*





























