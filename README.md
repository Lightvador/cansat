# cansat
#include <Wire.h>
#include <Adafruit_BMP085.h>
#include <SD.h>

// Create an instance of the BMP085 sensor
Adafruit_BMP085 bmp;

// Create a file to write the data to
File dataFile;

void setup() {
  Serial.begin(9600);

  // Initialize the BMP085 sensor
  if (!bmp.begin()) {
    Serial.println("Could not find a valid BMP085 sensor, check wiring!");
    while (1) {}
  }

  // Initialize the SD card
  if (!SD.begin(4)) {
    Serial.println("SD card initialization failed!");
    while (1) {}
  }

  // Open the file to write the data to
  dataFile = SD.open("altitude.txt", FILE_WRITE);
}

void loop() {
  // Read the altitude from the sensor
  float altitude = bmp.readAltitude();

  // Print the altitude to the serial monitor
  Serial.print("Altitude: ");
  Serial.print(altitude);
  Serial.println(" meters");

  // Write the altitude to the file
  dataFile.print("Altitude: ");
  dataFile.print(altitude);
  dataFile.println(" meters");

  // Wait for a bit before taking the next reading
  delay(1000);
}
