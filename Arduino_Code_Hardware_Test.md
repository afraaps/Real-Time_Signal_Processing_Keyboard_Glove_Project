###### Note: This version of the code to test is to allow you to test the hardware and the sensor values and ranges. You can use analogRead() to and this code to implement your own wireless verison with your own wifi protocol.

### Pins on ESP32 microcontroller to read analog input 
###### Note: the ESP32 turns off functionality for certain pins (all ADC2 pins) when enabling wifi on it so if you change any pins in the code made sure to double check the pinout. 

const int flexPin1 = 39; 
const int flexPin2 = 34; 
const int flexPin3 = 35; 
const int flexPin4 = 32; 
const int flexPin5 = 33; 

int value; 
int value2; 
int value3; 
int value4; 
int value5; 

void setup(){
  Serial.begin(115200); 
}

void loop(){
  value = analogRead(flexPin1);          //Read and save sensor value 
  Serial.print("Pinky"); 
  Serial.println(value);                 //Print value

  value2 = analogRead(flexPin2);         
  Serial.print("Ring: ");
  Serial.println(value2);       

  value3 = analogRead(flexPin3);         
  Serial.print("Middle: ");
  Serial.println(value3);       

  value4 = analogRead(flexPin4);        
  Serial.print("Pointer: ");
  Serial.println(value4);       

  value5 = analogRead(flexPin5);       
  Serial.print("Thumb: ");
  Serial.println(value5);       
  delay(100);                            //Small delay before next set of sensor readings
}
