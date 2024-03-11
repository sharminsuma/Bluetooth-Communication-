// Bluetooth-Communication-
//Bluetooth Communication Between Two Device (Master Device)

#include "BluetoothSerial.h"
BluetoothSerial SerialBT;
bool connect, state;

#include <LiquidCrystal_I2C.h>
#include <Wire.h> 

LiquidCrystal_I2C lcd(0x27,16,2);
#include <Keypad.h>

const byte ROWS = 4; //four rows
const byte COLS = 4; //four columns
//define the cymbols on the buttons of the keypads
char Keys[ROWS][COLS] = {
  {'1','2','3','A'},
  {'4','5','6','B'},
  {'7','8','9','C'},
  {'*','0','#','D'}
};
byte rowPins[ROWS] = {15, 16, 17, 4}; //connect to the row pinouts of the keypad
byte colPins[COLS] = {18, 19, 23, 5}; //connect to the column pinouts of the keypad

//initialize an instance of class NewKeypad
Keypad customKeypad = Keypad( makeKeymap(Keys), rowPins, colPins, ROWS, COLS); 

void setup(){
  Serial.begin(9600);
   lcd.init();
  lcd.backlight();
 
  SerialBT.begin("ESP32test", true);
  Serial.println("Master Mode Activated!");
  connect = SerialBT.connect("ESP32test");
  if(connect) Serial.println("Connected!");
  
}
  
void loop(){
  char customKey = customKeypad.getKey();
  
  if (customKey){
    lcd.clear();
    Serial.println(customKey);
    lcd.print(customKey);
    SerialBT.write(customKey);
    delay(20);  
  }
}










//Bluetooth Communication Between Two Device (Slave Device)

#include <BTAddress.h>
#include <BTAdvertisedDevice.h>
#include <BTScan.h>
#include <BluetoothSerial.h>
#include "BluetoothSerial.h"
BluetoothSerial SerialBT;
bool state;
#include <LiquidCrystal_I2C.h>
#include <Wire.h> 

LiquidCrystal_I2C lcd(0x27,16,2);
#include <Keypad.h>

const byte ROWS = 4; //four rows
const byte COLS = 4; //four columns
//define the cymbols on the buttons of the keypads
char Keys[ROWS][COLS] = {
  {'1','2','3','A'},
  {'4','5','6','B'},
  {'7','8','9','C'},
  {'*','0','#','D'}
};
byte rowPins[ROWS] = {15, 16, 17, 4}; //connect to the row pinouts of the keypad
byte colPins[COLS] = {18, 19, 23, 5}; //connect to the column pinouts of the keypad

//initialize an instance of class NewKeypad
Keypad customKeypad = Keypad( makeKeymap(Keys), rowPins, colPins, ROWS, COLS); 

void setup(){
  Serial.begin(9600);
   lcd.init();
  lcd.backlight();

  SerialBT.begin("ESP32test");
  Serial.println("Slave Mode Activated!");
}
  
void loop(){
  char customKey = customKeypad.getKey();
  
  if (customKey){
    lcd.clear();
    Serial.println(customKey);
    lcd.print(customKey);
  }
  if (SerialBT.available()) {
    lcd.clear();
    char sharmin = SerialBT.read();
    Serial.println(sharmin);
    lcd.print(sharmin);
    delay(20);
  }
}


