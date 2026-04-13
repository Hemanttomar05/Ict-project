# Ict-project
This about my ict workshop project which i made using arduino uno


👏 Clap Controlled Lamp using Arduino Nano

📌 Project Overview

This project demonstrates a simple home automation system where a lamp is controlled using sound (claps). The system uses an Arduino Nano, a sound sensor, and a relay module to detect clap patterns and toggle a lamp ON or OFF.

When two claps are detected within a short time interval, the Arduino processes the signal and switches the relay, allowing control of high-voltage devices like bulbs safely.

🎯 Objectives

To design a clap-based automation system
To understand sound sensor interfacing with Arduino
To control AC devices safely using a relay module
To implement timing logic (double clap detection) in Arduino
To build a low-cost and beginner-friendly IoT/home automation project


🧰 Components Used

Arduino Nano
Sound Sensor Module
5V Relay Module
Bulb + Bulb Holder
DC Power Supply (5V–9V)
Transistor (2A)
Diode (1N4148)
Resistors (SMD 102 ×3)
LEDs (Red & Green)
Female Headers
Terminal Connectors
DC Barrel Jack
PCB (optional)



🔌 Circuit Connections
Sound Sensor OUT → Arduino Digital Pin D3
Relay IN → Arduino Digital Pin D2
VCC (Sensor & Relay) → 5V
GND → GND
Relay Connections:
COM → AC Live Input
NO (Normally Open) → Bulb Live Terminal
Neutral → Direct to Bulb

⚠️ Warning: Be very careful while working with AC voltage.

💻 Arduino Code
#define relay 2
#define sensor 3

int clapCount = 0;
unsigned long firstClapTime = 0;
bool relayState = false;

void setup() {
  pinMode(sensor, INPUT);
  pinMode(relay, OUTPUT);
  Serial.begin(9600);
  digitalWrite(relay, HIGH); // Relay OFF (LOW = ON)
}

void loop() {
  bool pulse = digitalRead(sensor);

  if (pulse == 0) { // Sound detected
    Serial.println("Clap detected");

    if (clapCount == 0) {
      firstClapTime = millis();
      clapCount = 1;
    } 
    else if (clapCount == 1 && (millis() - firstClapTime) <= 1000) {
      clapCount = 2;
    }

    while (digitalRead(sensor) == 0);
    delay(50);
  }

  if (clapCount == 2) {
    relayState = !relayState;
    digitalWrite(relay, relayState ? LOW : HIGH);
    Serial.println(relayState ? "Relay ON" : "Relay OFF");
    clapCount = 0;
  }

  if (clapCount == 1 && (millis() - firstClapTime) > 1000) {
    clapCount = 0;
  }
}

🚀 How It Works

The sound sensor detects a clap and sends a signal to Arduino
Arduino checks for two claps within 1 second
If detected, it toggles the relay state
The relay switches the connected lamp ON/OFF

📷 Output
Clap twice → Lamp turns ON
Clap twice again → Lamp turns OFF

⚡ Applications
Smart home automation
Hands-free lighting systems
Assistive devices for physically challenged users
