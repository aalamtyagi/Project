#include <SoftwareSerial.h>

SoftwareSerial bluetooth(12, 13);
SoftwareSerial esp(3,4);
#define SSID "Redmi"
#define PASS "abcdefghijk"

int a,b;
String value;

String sendAT(String command, const int timeout)
{
  String response = "";
  esp.print(command);
  long int current_time = millis();
  while(millis()-current_time < timeout)
  {
    while(esp.available())
    {
      char c = esp.read();
       response += c;
       
    }
  }
  Serial.print(response);
  return response;
}


void setup() {
  bluetooth.begin(9600);  // put your setup code here, to run once:
esp.begin(9600); 
  Serial.begin(9600);
  
  sendAT("AT\r\n",1000);
  sendAT("AT+CWMODE=1\r\n",1000);
  sendAT("AT+CWJAP=\""SSID"\",\""PASS"\"\r\n",2000);
  while(!esp.find("OK"))
  {}
  sendAT("AT+CIFSR\r\n",1000);
  sendAT("AT+CIPMUX=0\r\n",1000);
  pinMode(8, OUTPUT);
  pinMode(9, OUTPUT);
  Serial.println("try to connect with bluetooth");
}

void loop() {
  bluetooth.listen();
 // Serial.println(value);
  if (bluetooth.available()) {
    value=bluetooth.readString(); 
      char bt;  
     if(value.length()>0)
     {
       Serial.println(value);
     
    if(value.equals("All light turn on"))
       {
        // Serial.println("it worked");
         digitalWrite(8, HIGH);
         digitalWrite(9, HIGH);
         a=1;
         b=1;         
       }
      if(value.equals("All light turn off"))
      {
        digitalWrite(8, LOW);
        digitalWrite(9, LOW);
        a=0;
        b=0;
      }
     if(value.equals("turn on light one"))
       {
          digitalWrite(8, HIGH);
          a=1;
       }  
      
     if(value.equals("turn off light one"))
       {
          digitalWrite(8, LOW);
          a=0;
       } 
      
     if(value.equals("turn on second light"))
       {
          digitalWrite(9, HIGH);
          b=1;
       } 
      
     if(value.equals("turn off second light"))
       {
          digitalWrite(9, LOW);
          b=0;
       } 
     } 
   /*  
    if(value=="a")
    {
       bt= 'a';
    }
     if(value=="b")
    {
       bt= 'b';
    }
      if(value=="A")
    {
       bt= 'A';
    }   
     if(value=="B")
    {
       bt= 'B';
    }
        switch (bt) {
      case 'A':
        digitalWrite(8, LOW);
        a=0;
        break;

      case 'a':
        digitalWrite(8, HIGH);
        a=1;
        break;
      case 'B':
        digitalWrite(9, LOW);
        b=0;
        break;
      case 'b':
        digitalWrite(9, HIGH);
        b=1;
        break;
    }
*/    
    String A = String(a);
    String B = String(b);
    updateTS(A,B);
  }  // put your main code here, to run repeatedly:
}

void updateTS(String A, String B)
{
  esp.listen();
  Serial.println("");
  sendAT("AT+CIPSTART=\"TCP\",\"api.thingspeak.com\",80\r\n",1000);
  String cmd = "GET /update?key=60UNBQ341JT17IIG&f1="+A+"&f2="+B+"\r\n";
  String cmdlen = String(cmd.length() );
  sendAT("AT+CIPSEND="+cmdlen+"\r\n",2000);
  esp.print(cmd);
  Serial.println("");
  sendAT("AT+CIPCLOSE\r\n",2000);
  Serial.println("");
}
