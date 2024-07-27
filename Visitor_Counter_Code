#include <ESP8266WiFi.h>
#include <WiFiClient.h>

#define BLYNK_TEMPLATE_ID "TMPL3C2mJILjW"
#define BLYNK_TEMPLATE_NAME "Visitor Counter"
#define BLYNK_AUTH_TOKEN "_d0hWe8gjf_4loTOb1NE3By3vFsAc4qL"

#include <BlynkSimpleEsp8266.h>
#include <Wire.h>
#include <Adafruit_GFX.h>
#include <Adafruit_SSD1306.h>

// Blynk auth token
char auth[] = "_d0hWe8gjf_4loTOb1NE3By3vFsAc4qL";

// WiFi credentials
char ssid[] = "WiFi Name";
char pass[] = "WiFi Password";

// OLED display
#define SCREEN_WIDTH 128
#define SCREEN_HEIGHT 64
#define OLED_RESET -1
Adafruit_SSD1306 display(SCREEN_WIDTH, SCREEN_HEIGHT, &Wire, OLED_RESET);

// Ultrasonic Sensor Pins
#define trigPin1 D6
#define echoPin1 D5
#define trigPin2 D8
#define echoPin2 D7

// Relay and Buzzer Pins
#define relay D3
#define BUZZER D4

// Visitor Count
int countin = 0;
int countout = 0;

WidgetLED light(V0);

// Function to get distance from ultrasonic sensor
long getDistance(int trigPin, int echoPin) {
  long duration, cm;
  digitalWrite(trigPin, LOW);
  delayMicroseconds(5);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);
  duration = pulseIn(echoPin, HIGH);
  cm = (duration / 2) / 29.1;
  return cm;
}

void setup() {
  Serial.begin(115200);
  Blynk.begin(auth, ssid, pass);
  
  pinMode(trigPin1, OUTPUT);
  pinMode(echoPin1, INPUT);
  pinMode(trigPin2, OUTPUT);
  pinMode(echoPin2, INPUT);
  pinMode(relay, OUTPUT);
  pinMode(BUZZER, OUTPUT);
  digitalWrite(relay, HIGH);
  digitalWrite(BUZZER, LOW);

  // Initialize the OLED display
  if (!display.begin(SSD1306_SWITCHCAPVCC, 0x3C)) {
    Serial.println(F("SSD1306 allocation failed"));
    for (;;); // Don't proceed, loop forever
  }

  display.clearDisplay();
  display.setTextSize(1);
  display.setTextColor(WHITE);
  display.setCursor(30, 20);
  display.print("Visitor");
  display.setCursor(30, 40);
  display.print("Counter");
  display.display();
  delay(3000);
}

void loop() {
  Blynk.run();

  long distance1 = getDistance(trigPin1, echoPin1);
  long distance2 = getDistance(trigPin2, echoPin2);

  Serial.print("Distance1: ");
  Serial.print(distance1);
  Serial.print(" cm, Distance2: ");
  Serial.print(distance2);
  Serial.println(" cm");

  static bool sensor1Triggered = false;
  static bool sensor2Triggered = false;
  static unsigned long sensor1TriggerTime = 0;
  static unsigned long sensor2TriggerTime = 0;
  static const unsigned long debounceDelay = 200; // debounce delay in milliseconds

  // Detect incoming visitors
  if (distance1 < 20 && !sensor1Triggered) {
    sensor1Triggered = true;
    sensor1TriggerTime = millis();
  }

  if (distance2 < 20 && !sensor2Triggered) {
    sensor2Triggered = true;
    sensor2TriggerTime = millis();
  }

  if (sensor1Triggered && sensor2Triggered) {
    if (sensor1TriggerTime < sensor2TriggerTime && (millis() - sensor1TriggerTime > debounceDelay)) {
      countin++;
      digitalWrite(BUZZER, HIGH);
      delay(200);
      digitalWrite(BUZZER, LOW);
      sensor1Triggered = false;
      sensor2Triggered = false;
      Serial.println("Visitor Entered");
    } else if (sensor2TriggerTime < sensor1TriggerTime && (millis() - sensor2TriggerTime > debounceDelay)) {
      countout++;
      digitalWrite(BUZZER, HIGH);
      delay(200);
      digitalWrite(BUZZER, LOW);
      sensor1Triggered = false;
      sensor2Triggered = false;
      Serial.println("Visitor Exited");
    }
  }

  // Reset sensor triggers if both sensors are not triggered for a while
  if (distance1 >= 20 && distance2 >= 20) {
    sensor1Triggered = false;
    sensor2Triggered = false;
  }

  int now = countin - countout;

  if (now <= 0) {
    light.off();
    digitalWrite(relay, HIGH);
    display.clearDisplay();
    display.setTextSize(1);
    display.setTextColor(WHITE);
    display.setCursor(30, 15);
    display.print("No Visitor");
    display.setCursor(30, 35);
    display.print("Light Off");
    display.display();
    Serial.println("No Visitors! Light Off");
  } else {
    light.on();
    digitalWrite(relay, LOW);
    display.clearDisplay();
    display.setTextColor(WHITE);
    display.setTextSize(1);
    display.setCursor(15, 0);
    display.print("Current Visitor");

    display.setTextSize(2);
    display.setCursor(50, 15);
    display.print(now);

    display.setTextSize(1);
    display.setCursor(0, 40);
    display.print("IN: ");
    display.print(countin);

    display.setCursor(70, 40);
    display.print("OUT: ");
    display.print(countout);

    display.display();
    Serial.print("Current Visitor: ");
    Serial.println(now);
    Serial.print("IN: ");
    Serial.println(countin);
    Serial.print("OUT: ");
    Serial.println(countout);
  }

  Blynk.virtualWrite(V1, countin);
  Blynk.virtualWrite(V2, countout);
  Blynk.virtualWrite(V3, now);
  delay(1000);
}
