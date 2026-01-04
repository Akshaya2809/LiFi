# LiFi
This repo has the codes of my Light Fidelity project both transmitter and receiver sides.
<br>
# This is for transmitter side
#include <Keypad.h>

/* ---------- Keypad Configuration ---------- */
const byte ROWS = 4;
const byte COLS = 4;

char keys[ROWS][COLS] = {
  {'1','2','3','A'},
  {'4','5','6','B'},
  {'7','8','9','C'},
  {'*','0','#','D'}
};

byte rowPins[ROWS] = {A5, A4, A3, A2};   // R1, R2, R3, R4
byte colPins[COLS] = {A1, A0, 12, 11};   // C1, C2, C3, C4

Keypad keypad = Keypad(makeKeymap(keys), rowPins, colPins, ROWS, COLS);

/* ---------- Setup ---------- */
void setup() {
  pinMode(8, OUTPUT);      // LED output
  digitalWrite(8, LOW);    // LED OFF initially
}

/* ---------- Loop ---------- */
void loop() {
  char key = keypad.getKey();

  if (key) {
    digitalWrite(8, HIGH);

    // Unique pulse duration for each key
    switch (key) {
      case '1': delay(12); break;
      case '2': delay(14); break;
      case '3': delay(16); break;
      case 'A': delay(18); break;

      case '4': delay(20); break;
      case '5': delay(22); break;
      case '6': delay(24); break;
      case 'B': delay(26); break;

      case '7': delay(28); break;
      case '8': delay(30); break;
      case '9': delay(32); break;
      case 'C': delay(34); break;

      case '*': delay(36); break;
      case '0': delay(38); break;
      case '#': delay(40); break;
      case 'D': delay(42); break;
    }

    digitalWrite(8, LOW);
    delay(200);   // Gap between pulses
  }
}
<br>
# This is for the receiver side
#include <Wire.h>
#include <LiquidCrystal_I2C.h>

/* ---------- LCD Configuration ---------- */
LiquidCrystal_I2C lcd(0x3F, 16, 2);

/* ---------- Setup ---------- */
void setup() {
  pinMode(8, INPUT);     // LDR input
  Serial.begin(9600);

  lcd.init();
  lcd.backlight();

  lcd.setCursor(0, 0);
  lcd.print("  WELCOME TO  ");
  lcd.setCursor(0, 1);
  lcd.print("  LI-FI DEMO  ");
  delay(2000);
  lcd.clear();
}

/* ---------- Loop ---------- */
void loop() {
  unsigned long duration = pulseIn(8, HIGH);
  Serial.println(duration);

  lcd.setCursor(0, 0);
  lcd.print("Received: ");

  if (duration > 10000 && duration < 13000) lcd.print("1 ");
  else if (duration > 13000 && duration < 15000) lcd.print("2 ");
  else if (duration > 15000 && duration < 17000) lcd.print("3 ");
  else if (duration > 17000 && duration < 19000) lcd.print("A ");

  else if (duration > 19000 && duration < 21000) lcd.print("4 ");
  else if (duration > 21000 && duration < 23000) lcd.print("5 ");
  else if (duration > 23000 && duration < 25000) lcd.print("6 ");
  else if (duration > 25000 && duration < 27000) lcd.print("B ");

  else if (duration > 27000 && duration < 29000) lcd.print("7 ");
  else if (duration > 29000 && duration < 31000) lcd.print("8 ");
  else if (duration > 31000 && duration < 33000) lcd.print("9 ");
  else if (duration > 33000 && duration < 35000) lcd.print("C ");

  else if (duration > 35000 && duration < 37000) lcd.print("* ");
  else if (duration > 37000 && duration < 39000) lcd.print("0 ");
  else if (duration > 39000 && duration < 41000) lcd.print("# ");
  else if (duration > 41000 && duration < 44000) lcd.print("D ");
  else lcd.print("----");

  delay(300);
}

