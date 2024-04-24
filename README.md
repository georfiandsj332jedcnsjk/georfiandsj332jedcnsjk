- 👋 Hi, I’m @georfiandsj332jedcnsjk
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...

<!---
georfiandsj332jedcnsjk/georfiandsj332jedcnsjk is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->

#include <ESP8266WiFi.h>
#include <PubSubClient.h>
#include <SoftwareSerial.h>
#include <ArduinoJson.h>
const char *ssid = "DIGI-T8CD";
const char *password = "62hGeg2r";
const char *mqtt_broker = "broker.emqx.io";
const char *publish_topic = "/EA4/licenta/parcare";
const char *mqtt_username = "emqx";
const char *mqtt_password = "public";
const int mqtt_port = 1883;
WiFiClient wifiClient;
PubSubClient mqttClient(wifiClient);
void setup_wifi() {
delay(10);
Serial.print("\nConnecting to ");
Serial.println(ssid);
WiFi.mode(WIFI_STA);
WiFi.begin(ssid, password);
while (WiFi.status() != WL_CONNECTED) {
delay(500);
Serial.print(".");
}
randomSeed(micros());
Serial.println("\nWiFi connected\nIP address: ");
Serial.println(WiFi.localIP());
}
void reconnect() {
while (!mqttClient.connected()) {
Serial.print("Attempting MQTT connection...");
String clientId = "ESP8266Client-";
clientId += String(random(0xffff), HEX);
if (mqttClient.connect(clientId.c_str(), mqtt_username, mqtt_password)) {
Serial.println("connected");
} else {
Serial.print("failed, rc=");
Serial.print(mqttClient.state());
Serial.println("try again in 5 seconds");
delay(5000);
}
}
}
void publishMessage(const char* topic, String payload, boolean retained) {
if (mqttClient.publish(topic, payload.c_str(), true))
Serial.println("Message published " + String(topic) + ": " + payload);
}
float readDistanceFromSensor1() {
// Implementați citirea distanței de la senzorul 1
return 0.0; // Returnați distanța citită
}
float readDistanceFromSensor2() {
// Implementați citirea distanței de la senzorul 2
return 0.0; // Returnați distanța citită
}
float readDistanceFromSensor3() {
// Implementați citirea distanței de la senzorul 3
return 0.0; // Returnați distanța citită
}
float readDistanceFromSensor4() {
// Implementați citirea distanței de la senzorul 4
return 0.0; // Returnați distanța citită
}
void setup() {
Serial.begin(9600);
while (!Serial) delay(1);
setup_wifi();
mqttClient.setServer(mqtt_broker, mqtt_port);
}
void loop() {
 if (!client.connected()) {
    reconnect();
  }
  client.loop();
float distance1 = readDistanceFromSensor1();
float distance2 = readDistanceFromSensor2();
float distance3 = readDistanceFromSensor3();
float distance4 = readDistanceFromSensor4();
DynamicJsonDocument doc(1024);
doc["deviceId"] = "NodeMCU";
doc["siteId" ]= "My Demo Lab";
doc["distance1" ]= distance1;
doc["distance2" ]= distance2;
doc["distance3"]= distance3;
doc["distance4"] = distance4;
char mqtt_message128;
serializeJson(doc, mqtt_message);
publishMessage("/EA4/licenta/parcare", mqtt_message, true);
delay(5000);
}
