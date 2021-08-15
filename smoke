#include <Arduino.h>
#include <ESP8266WiFi.h>

//String apiKey = "RITEYV25KKNP04P8";
const char* ssid = "K";
const char* password = "Aksaksaksaks";
const char* server = "10.2.73.163";  //api.thinkspeak.com
const int postingInterval = 20 * 1000; //post data every 20 seconds
//const int channelID = 700689;
int sensorPin = A0;


int enableMQ2 = 13;    //D7
int PinSignal = 5;   //D1
int temperature;
int sensorThres = 1;


WiFiClient client;

void setup()
{
    Serial.begin(115200);

    pinMode(sensorPin, INPUT);
    //pinMode(enableTMP, OUTPUT);
    pinMode(enableMQ2, INPUT);
    pinMode(PinSignal, OUTPUT);

    //--------------------------Wifi-------------------------

    Serial.println();
    Serial.print("Connecting to ");
    Serial.println(ssid);
    Serial.print("..........");
    Serial.println();
    WiFi.begin(ssid, password);
    while (WiFi.status() != WL_CONNECTED)
    {
        delay(500);
        Serial.print(".");
    }

    Serial.println("WiFi connected");
    Serial.println();

}


void loop()
{
    //--------------------------TMP36 Sensor-------------------------

    double reading = analogRead(sensorPin);

      double voltage= ((reading*3.3)/ 1024);
      temperature= (voltage*100);
      Serial.print("Temperature:    ");
      Serial.print(temperature);
      Serial.print("      ");


    //--------------------------MQ2 Sensor-------------------------

    int SensorReading = 1 - digitalRead(enableMQ2);
    Serial.print("SensorReading:      ");
    Serial.println(SensorReading);
    delay(100);
    if (temperature > 0 &&  SensorReading == sensorThres)
    {
        digitalWrite(PinSignal,HIGH);    //tone(pin, frequency, duration)
        tone(PinSignal, 1000, 200);
    }
    else
    {
        digitalWrite(PinSignal,LOW);
    }




    //--------------------------thingspeak-------------------------

    if (client.connect(server,8080))
    {
          // "184.106.153.149" or api.thingspeak.com
        Serial.print("connected");
        String postStr="";// = apiKey;
        postStr +="field1=";
        postStr += String(temperature);

        postStr +="&field2=";
        postStr += String(SensorReading);

        postStr += "\r\n\r\n\r\n\r\n\r\n\r\n\r\n\r\n";

        client.print("POST /update HTTP/1.1\n");
        client.print("Host: 10.2.73.163\n");
        client.print("Connection: close\n");
        //client.print("X-THINGSPEAKAPIKEY: "+apiKey+"\n");
        client.print("Content-Type: application/x-www-form-urlencoded\n");
        client.print("Content-Length: ");
        client.print(postStr.length());
        client.print("\n\n\n\n\n\n\n\n");
        client.print(postStr);
    }
    client.stop();
    //delay(postingInterval);
}
