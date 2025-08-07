#include <SPI.h>
#include <MFRC522.h>
#include <Wire.h>
#include <LiquidCrystal_I2C.h>

#define SS_PIN 10
#define RST_PIN 9
#define LED_DENIED_PIN 5
#define LED_ACCESS_PIN 4
#define BUZZER 2
#define LOCK_PIN 3 

LiquidCrystal_I2C lcd(0x3F, 16, 2);
MFRC522 mfrc522(SS_PIN, RST_PIN); 
byte code[] = {0x23, 0xDC, 0x58, 0x90};
bool doorUnlocked = false;

void setup() {
    Serial.begin(9600);
    SPI.begin();        
    mfrc522.PCD_Init(); 
    Serial.println("Arduino RFID reading UID");
    pinMode(LED_DENIED_PIN, OUTPUT);
    pinMode(LED_ACCESS_PIN, OUTPUT);
    pinMode(LOCK_PIN, OUTPUT); 
    digitalWrite(LOCK_PIN, HIGH); 
    lcd.init();
    lcd.backlight();
    lcd.setCursor(0, 0);
    lcd.print(" Scan Your RFID");
    lcd.setCursor(0, 1);
    lcd.print("  Door Locked");
}

void loop() {
    if (!mfrc522.PICC_IsNewCardPresent()) {
        return;
    }
    if (!mfrc522.PICC_ReadCardSerial()) {
        return;
    }
    Serial.print("UID tag :");
    String content = "";
    for (byte i = 0; i < mfrc522.uid.size; i++) {
        Serial.print(mfrc522.uid.uidByte[i] < 0x10 ? " 0" : " ");
        Serial.print(mfrc522.uid.uidByte[i], HEX);
        content.concat(String(mfrc522.uid.uidByte[i] < 0x10 ? " 0" : " "));
        content.concat(String(mfrc522.uid.uidByte[i], HEX));
    }
    Serial.println();
    Serial.print("Message : ");
    content.toUpperCase();
    if (content.substring(1) == "23 DC 58 90") // Change here the UID of card/cards or tag/tags that you want to give access
    {
        unlockDoor();
    } else {
        lockDoor();
    }
}

void unlockDoor() {
    Serial.println("Access Granted");
    Serial.println();
    delay(500);
    digitalWrite(LED_ACCESS_PIN, HIGH);
    lcd.clear();
    lcd.setCursor(0, 1);
    lcd.print(" Door Un-Locked");
    tone(BUZZER, 2000);
    delay(100);
    noTone(BUZZER);
    delay(50);
    tone(BUZZER, 2000);
    delay(100);
    noTone(BUZZER);
    digitalWrite(LOCK_PIN, LOW); 
    doorUnlocked = true;
    delay(5000); // Wait for 5 seconds
    digitalWrite(LOCK_PIN, HIGH); 
    doorUnlocked = false; 
    digitalWrite(LED_ACCESS_PIN, LOW); 
    lcd.clear();
    lcd.setCursor(0, 1); 
    lcd.print("  Door Locked ");
    tone(BUZZER, 2000);
    delay(100);
    noTone(BUZZER);
    delay(50);
}

void lockDoor() {
    lcd.clear();
    lcd.setCursor(0, 1); 
    lcd.print("Invalid RFID Tag");
    Serial.println(" Access denied");
    digitalWrite(LED_DENIED_PIN, HIGH);
    tone(BUZZER, 1500);
    delay(500);
    digitalWrite(LED_DENIED_PIN, LOW);
    noTone(BUZZER);
    delay(100);
    digitalWrite(LED_DENIED_PIN, HIGH);
    tone(BUZZER, 1500);
    delay(500);
    digitalWrite(LED_DENIED_PIN, LOW);
    noTone(BUZZER);
    delay(100);
    digitalWrite(LED_DENIED_PIN, HIGH);
    tone(BUZZER, 1500);
    delay(500);
    digitalWrite(LED_DENIED_PIN, LOW);
    noTone(BUZZER);
    lcd.clear();
    lcd.setCursor(0, 1); 
    lcd.print("   Door Locked  ");
}
