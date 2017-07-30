#include <ArduinoJson.h>
#include <LWiFi.h>
#include <PubSubClient.h>

char ssid[] = "SSID";
char password[] = "Password";
char mqtt_server[] = "MQTT Broker IP address";
char sub_topic[] = "7697sub-mixer";
char pub_topic[] = "7697pub-mixer";
char client_Id[] = "7697client-Mixer";
int A = 14;
int B = 15;
int C = 16;
int D = 17;
int At = 0;
int Bt = 0;
int Ct = 0;
int Dt = 0;

int status = WL_IDLE_STATUS;

WiFiClient mtclient;      //從WiFiClient 產生 mtclient的物件
PubSubClient client(mtclient);
long lastMsg = 0;
char msg[50];
int value = 0;

void setup() {
       
    //Relay{
    pinMode(14, OUTPUT);
    digitalWrite(14, HIGH);
    pinMode(15, OUTPUT);
    digitalWrite(15, HIGH);
    pinMode(16, OUTPUT);
    digitalWrite(16, HIGH);
    pinMode(17, OUTPUT); 
    digitalWrite(17, HIGH);
        
    //Initialize serial and wait for port to open:
    Serial.begin(9600);
    //while (!Serial) {
        ; // wait for serial port to connect. Needed for native USB port only
    //}
    setup_wifi();
    client.setServer(mqtt_server, 1883);
    client.setCallback(callback); 

}

void setup_wifi() {
   // attempt to connect to Wifi network:
   Serial.print("Attempting to connect to SSID: ");
   Serial.println(ssid);
   WiFi.begin(ssid,password);
   while (WiFi.status() != WL_CONNECTED) {
     delay(500);
     Serial.print(".");
    }
    randomSeed(micros());
    Serial.println("Connected to wifi");
    printWifiStatus();
}


void callback(char* topic, byte* payload, unsigned int length) {
   for (int i = 0; i < length; i++) {
    Serial.print((char)payload[i]);
  }
 
 //Json{
  StaticJsonBuffer<1000> jsonBuffer;
  JsonObject& root = jsonBuffer.parseObject(payload);
  int A = root["A"];
  int B = root["B"];
  int C = root["C"];
  int D = root["D"];
  int At = int(root["At"]) * 1000;
  int Bt = int(root["Bt"]) * 1000;
  int Ct = int(root["Ct"]) * 1000;
  int Dt = int(root["Dt"]) * 1000;
  Serial.println(A);
  Serial.println(B);
  Serial.println(C);
  Serial.println(D);
  Serial.println(At);
  Serial.println(Bt);
  Serial.println(Ct);
  Serial.println(Dt);
  client.publish(pub_topic, "Mix start");
  digitalWrite(A, LOW); 
  delay(At);
  digitalWrite(A, HIGH); 
  digitalWrite(B, LOW); 
  delay(Bt);
  digitalWrite(B, HIGH);
  digitalWrite(C, LOW); 
  delay(Ct);
  digitalWrite(C, HIGH);
  digitalWrite(D, LOW); 
  delay(Dt);
  digitalWrite(D, HIGH);
  delay(500);
  client.publish(pub_topic, "Mix finsh");

}

void reconnect() {
  // Loop until we're reconnected
  while (!client.connected()) {
    Serial.print("Attempting MQTT connection...");
    // Create a random client ID
    String clientId = "MT76976Client-";
    clientId += String(random(0xffff), HEX);
    // Attempt to connect
    if (client.connect(clientId.c_str())) {
      Serial.println("connected");
      // Once connected, publish an announcement...
      client.publish(pub_topic, "Mix finsh");
      // ... and resubscribe
      client.subscribe(sub_topic);
    } else {
      Serial.print("failed, rc=");
      Serial.print(client.state());
      Serial.println(" try again in 5 seconds");
      // Wait 5 seconds before retrying
      delay(5000);
    }
  }
}


void loop() {
  if (!client.connected()) {
    reconnect();
  }
  client.loop();

}


void printWifiStatus() {
    // print the SSID of the network you're attached to:
    Serial.print("SSID: ");
    Serial.println(WiFi.SSID());

    // print your WiFi shield's IP address:
    IPAddress ip = WiFi.localIP();
    Serial.print("IP Address: ");
    Serial.println(ip);

    // print the received signal strength:
    long rssi = WiFi.RSSI();
    Serial.print("signal strength (RSSI):");
    Serial.print(rssi);
    Serial.println(" dBm");
}
