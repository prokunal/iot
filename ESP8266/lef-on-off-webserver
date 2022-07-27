#include<ESP8266WiFi.h>

const char* ssid = "OSCP_4G";
const char* password = "OL7Looters@";
String header;
String outputd1state = "off";
const int outputd1 = D1;

WiFiServer server(80);

void setup()
{
  Serial.begin(115200);
  pinMode(outputd1, OUTPUT);
  digitalWrite(outputd1,LOW);
  

  Serial.print("Connecting to ");
  Serial.println(ssid);
  WiFi.begin(ssid,password);
  while(WiFi.status() != WL_CONNECTED)
  {
    delay(500);
    Serial.print(".");
  }


  Serial.println("");
  Serial.println("WiFi connected--> ");
  Serial.println("Copy this IP address: ");
  Serial.println(WiFi.localIP());
  server.begin();
}

void loop()
{
  WiFiClient client = server.available(); // Listen for incoming clients
  if(client)
  {

    Serial.println("new client.");
    String currentLine = "";
    while(client.connected())
    {
      if(client.available())
      {
      char c = client.read();
      Serial.write(c);
      header +=c;
      if(c == '\n')
      {
        if(currentLine.length() == 0)
        {
          client.println("HTTP/1.1 200 OK");
          client.println("Content-type:text/html");
          client.println("Connection: close");
          client.println();

          if(header.indexOf("GET /d1/on") >= 0)
          {
            Serial.println("GPIO D1 ON");
            outputd1state = "on";
            digitalWrite(outputd1, HIGH);
          }
          else if(header.indexOf("GET /d1/off") >=0)
          {
            Serial.println("GPIO D1 OFF");
            outputd1state = "off";
            digitalWrite(outputd1, LOW);
          }

          client.println("<!DOCTYPE html><html>");
          client.println("<head><meta name\"viewport\" content=\"width=device-width,initial-scale=1\"> ");
          client.println("<link rel=\"icon\" href=\"data:,\">");
          client.println("<style>html {font-family:Helvetica; display:inline-block;margin:0px auto; text-align: center;}");
          client.println("text-decoration: none;font-size: 30px; margin: 2px; cursor: pointer;}");
          client.println(".button2 {background-color: #77878A;}</style></head>");
          client.println("<body><h1>ESP8266 WEB SERVER</h1>");

          client.println("<p>D1 - STATE "+outputd1state+"</p>");
          if(outputd1state=="off")
          {
            client.println("<p><a href=\"/d1/on\"><button class=\"button\">ON</button></a></p>");
          }
          else
          {
            client.println("<p><a href=\"/d1/off\"><button class=\"button2\">OFF</button></a></p>");
          }
          client.println("</body></html>");
          client.println();
          break;
        }
        else{
          currentLine = "";
        }
      }
      else if(c != '\r'){
        currentLine += c;
      }
    }
  }
  header = "";
  client.stop();
  Serial.println("Client disconnected.");
  Serial.println("");
}
}

