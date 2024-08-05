#include <LiquidCrystal.h>// LCD module connections (change accordingly to your setup)
const int rs = 12, en = 11, d4 = 5, d5 = 4, d6 = 3, d7 = 2, ct=9;
LiquidCrystal lcd(rs, en, d4, d5, d6, d7);
// MQ-2 Gas Sensor pin
const int mq2Pin = A0;
const int buzzer = 9;
// Your threshold value. You might need to change it.
int sensorThres = 200;
void setup() {
  lcd.begin(16, 2); // Initialize the LCD
  pinMode(mq2Pin, INPUT);
  pinMode(buzzer, OUTPUT);
  Serial.begin(9600); // Initialize Serial communication
  while (!Serial) {
    ; // Wait for Serial to be ready
  }
  lcd.print("Monitoring..."); 
}
void loop() {
  // Read pollution value from the MQ-2 sensor
  int pollutionValue = analogRead(mq2Pin);
  // Display pollution value on the LCD
  lcd.setCursor(0, 1);
  lcd.print("Pollution: ");
  lcd.print(pollutionValue);
  // Send data to the laptop via Serial communication
  Serial.print("Value: ");
  Serial.println(pollutionValue);
  //send signal to buzzer
  if (pollutionValue > sensorThres) {
    tone(buzzer, 1000, 1000);
  } else {
    noTone(buzzer);
  }
 delay(1000); // Delay between readings (adjust as needed)
}
