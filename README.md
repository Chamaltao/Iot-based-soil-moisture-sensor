   #include <ESP8266WiFi.h>
  #define myPeriodic 15 //in sec | Thingspeak pub is 15sec
  
  
  const char* server = "api.thingspeak.com";
  String apiKey ="6T36C0206TXHPYT8";
  const char* MY_SSID = "CMEC"; 
  const char* MY_PWD = "C123456L";
  int sent = 0;
  
  void setup() {
  
    
  connectWifi();
  }
  
  void loop() {
  
    float soilData =analogRead(A0);
  
  
  
    send_Data(soilData,"&field1=");
    int count = myPeriodic;
    while(count--)
    delay(1000);
    
   
 }
  
  void connectWifi()
  {
    //Serial.print("Connecting to "+*MY_SSID);
    WiFi.begin(MY_SSID, MY_PWD);
    while (WiFi.status() != WL_CONNECTED) {
    delay(1000);
    //Serial.print(".");
    }
    
    //Serial.println("");
   // Serial.println("Connected");
    //Serial.println("");  
  }//end connect
  
  void send_Data(float temp,String field)
  {  
     WiFiClient client;
    
     if (client.connect(server, 80)) { // use ip 184.106.153.149 or api.thingspeak.com
     //Serial.println("WiFi Client connected ");
     
     String postStr = apiKey;
     postStr += field;
     postStr += String(temp);
     postStr += "\r\n\r\n";
     
     client.print("POST /update HTTP/1.1\n");
     client.print("Host: api.thingspeak.com\n");
     client.print("Connection: close\n");
     client.print("X-THINGSPEAKAPIKEY: " + apiKey + "\n");
     client.print("Content-Type: application/x-www-form-urlencoded\n");
     client.print("Content-Length: ");
     client.print(postStr.length());
     client.print("\n\n");
     client.print(postStr);
     delay(1000);
     
     }//end if
     sent++;
   client.stop();
  }//end send
