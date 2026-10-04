#include <Wire.h>
#include <LiquidCrystal_I2C.h>
#include <Servo.h>

// ================= LCD =================
LiquidCrystal_I2C lcd(0x27, 16, 2);

// ================= IR SENSORS =================
// Parking slots
#define IR1 2
#define IR2 3
#define IR3 4
#define IR4 5

// Entry and Exit
#define ENTRY_IR 6
#define EXIT_IR 7

// ================= SERVOS =================
#define ENTRY_SERVO 9
#define EXIT_SERVO 10

Servo entryServo;
Servo exitServo;

// Most IR obstacle sensors give LOW when object is detected
#define CAR_DETECTED LOW

void setup() {

  // Serial monitor
  Serial.begin(9600);

  // IR sensors
  pinMode(IR1, INPUT);
  pinMode(IR2, INPUT);
  pinMode(IR3, INPUT);
  pinMode(IR4, INPUT);
  pinMode(ENTRY_IR, INPUT);
  pinMode(EXIT_IR, INPUT);

  // Servos
  entryServo.attach(ENTRY_SERVO);
  exitServo.attach(EXIT_SERVO);

  // Start barriers closed
  entryServo.write(0);
  exitServo.write(0);

  // I2C LCD
  // Arduino UNO: SDA = A4, SCL = A5
  Wire.begin();

  lcd.init();
  lcd.backlight();

  lcd.setCursor(0, 0);
  lcd.print("SMART PARKING");

  lcd.setCursor(0, 1);
  lcd.print("SYSTEM READY");

  delay(2000);
  lcd.clear();
}

void loop() {

  // ================= READ PARKING SENSORS =================

  bool s1 = digitalRead(IR1) == CAR_DETECTED;
  bool s2 = digitalRead(IR2) == CAR_DETECTED;
  bool s3 = digitalRead(IR3) == CAR_DETECTED;
  bool s4 = digitalRead(IR4) == CAR_DETECTED;

  // Count occupied slots
  int occupied = 0;

  if (s1) occupied++;
  if (s2) occupied++;
  if (s3) occupied++;
  if (s4) occupied++;

  int freeSlots = 4 - occupied;

  // ================= LCD DISPLAY =================

  lcd.setCursor(0, 0);

  lcd.print("S1:");
  lcd.print(s1 ? "X" : "O");

  lcd.print(" S2:");
  lcd.print(s2 ? "X" : "O");

  lcd.print(" S3:");
  lcd.print(s3 ? "X" : "O");

  lcd.setCursor(0, 1);

  lcd.print("S4:");
  lcd.print(s4 ? "X" : "O");

  lcd.print(" Free:");
  lcd.print(freeSlots);

  lcd.print(" ");

  // ================= ENTRY =================

  if (digitalRead(ENTRY_IR) == CAR_DETECTED) {

    if (freeSlots > 0) {

      lcd.clear();

      lcd.setCursor(0, 0);
      lcd.print("WELCOME!");

      lcd.setCursor(0, 1);
      lcd.print("FREE SLOTS:");
      lcd.print(freeSlots);

      // Open entry barrier
      entryServo.write(90);

      delay(3000);

      // Close entry barrier
      entryServo.write(0);

      lcd.clear();
    }

    else {

      lcd.clear();

      lcd.setCursor(0, 0);
      lcd.print("PARKING FULL");

      lcd.setCursor(0, 1);
      lcd.print("NO SPACE!");

      delay(2000);

      lcd.clear();
    }
  }

  // ================= EXIT =================

  if (digitalRead(EXIT_IR) == CAR_DETECTED) {

    lcd.clear();

    lcd.setCursor(0, 0);
    lcd.print("THANK YOU!");

    lcd.setCursor(0, 1);
    lcd.print("HAVE A NICE DAY");

    // Open exit barrier
    exitServo.write(90);

    delay(3000);

    // Close exit barrier
    exitServo.write(0);

    lcd.clear();
  }

  delay(200);
}
