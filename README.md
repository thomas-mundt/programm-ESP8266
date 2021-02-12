# How To Programm ESP8266

## Install Arduino IDE

- https://www.arduino.cc/en/software


## Install The ESP8266 Software

- https://github.com/esp8266/Arduino  


Arduino/Einstellungen/Zusätzliche Bordverwalter-URLs
```
https://arduino.esp8266.com/stable/package_esp8266com_index.json
```

Werkzeuge/Board



## Install Device Drivers

- http://www.wch.cn/downloads/file/178.html?time=2021-02-12%2000:44:15&code=JkUrO5ilnjZKgNXTaAxeTzWC6QRkCdEPT4Mm0iwg

## Running An Example


Connect to wlan and grep webpage
```
#include <ESP8266WiFi.h>

// WiFi parameters
const char * ssid = "xxx";
const char * password = "xxx";

// Host
const char * host = "www.example.com";

void setup() {
  // Start Serial
  Serial.begin(115200);

  // We start by connecting to a WiFi network
  Serial.println();
  Serial.println();
  Serial.print("Connecting to ");
  Serial.println(ssid);
  
  WiFi.begin(ssid, password);
  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }

  Serial.println("");
  Serial.println("WiFi connected");
  Serial.println("IP address: ");
  Serial.println(WiFi.localIP());
}

int value = 0;

void loop() {

  Serial.print("Connecting to ");
  Serial.println(host);

  // Use WiFiClient class to create TCP connections
  WiFiClient client;
  const int httpPort = 80;
  if (!client.connect(host, httpPort)) {
    Serial.println("connection failed");
    return;
  }

  String url = "/";
  Serial.print("Requested URL: ");
  Serial.println(url);

  // This will send the request to the server
  client.print(String("GET ") + url + " HTTP/1.1\r\n" + "Host: " + host + "\r\n" + "Connection: close\r\n\r\n");


  // start waiting for the response             
  unsigned long lasttime = millis();
  while (!client.available() && millis() - lasttime < 1000) {delay(1);}   // wait max 1s for data



  // Read all the lines of the reply from server and print them to Serial
  while(client.available()){
    String line = client.readStringUntil('\r');
    Serial.print(line);
  }

  delay(5000);

}
```


