#include <ESP8266HTTPClient.h>
#include <ESP8266WiFi.h>
#include <ArduinoJson.h>

const char* ssid     = "Your home Wifi Ssid";// Your home Wifi Ssid
const char* password = "password"; //Home wifi password
const char* host = "192.168.1.155"; //on localhost use ur computer ip or any Host ex(www.example.com)
const char*  path = "/nodemcu/nodemcu.json";//folder path

void setup() {
  Serial.begin(115200); //Serial connection
  WiFi.begin(ssid, password);   //WiFi connection
  while (WiFi.status() != WL_CONNECTED) {  //Wait for the WiFI connection completion
    delay(500);
    Serial.println("Waiting for connection");
  }
}

void loop() {
   String link =String("http://") + String(host) + path;
   if(WiFi.status()== WL_CONNECTED){   //Check WiFi connection status
   HTTPClient http;    //Declare object of class HTTPClient
   http.begin(link);      //Specify request destination
   http.addHeader("Content-Type", "application/x-www-form-urlencoded");  //Specify content-type header
   int httpCode = http.POST(link);   //Send the request
   String payload = http.getString();                  //Get the response payload
   //Serial.println(httpCode);   //Print HTTP return code
   Serial.println(payload);    //Print request response payload
   String result = payload;
   // Parse JSON
      int size = result.length() + 1;
      char json[size];
      result.toCharArray(json, size);
      StaticJsonBuffer<200> jsonBuffer;
      JsonObject& json_parsed = jsonBuffer.parseObject(json);
      if (!json_parsed.success())
      {
        Serial.println("parseObject() failed");
        return;
      }
          int pins [] = {4,5,14,12,13,15,10};
          int pinCount = 7;
          for (int thisPin = 0; thisPin < pinCount; thisPin++) {
              pinMode(pins[thisPin], OUTPUT);
              //Serial.println(pins[thisPin]);
             String pin = String(pins[thisPin]);
             Serial.println(pin);
             if(strcmp(json_parsed[pin], "on") == 0){
                Serial.println("Device ON"); 
                digitalWrite(pins[thisPin], HIGH);
               }else{
                Serial.println("Device OF"); 
                digitalWrite(pins[thisPin], LOW);
               }//if json
            }//for loop            
      http.end();  //Close connection
 }else{
    Serial.println("Error in WiFi connection");   
 }
   delay(1000);
}
