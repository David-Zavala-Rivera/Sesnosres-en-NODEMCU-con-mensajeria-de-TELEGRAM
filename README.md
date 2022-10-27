# Senosres-en-NODEMCU-con-mensajeria-de-TELEGRAM

En este código presento un programa el cual manda alerta por medio de Telegram cuando los sensores se encuentran inactivos, lo unico que hay que realizar es colocar la SSID, el PASS de la red a la cual se debe conectar, los TOKEN y los ID a quienes se va a notificar

```
//bibliotecas para ESP8266
#include <ESP8266WiFi.h>
#include <WiFiClientSecure.h>


//bibliotecas para ESP8266
#include <ESP8266WiFi.h>
#include <WiFiClientSecure.h>


//Bibliotecas para TELEGRAM
#include <UniversalTelegramBot.h>
#include <ArduinoJson.h>

//PINES de ESP8266
#define boton1 16
#define boton2 5
#define interruptor 4

// funcion Deep Sleep
#define SLEEP_TIME 1800

//Variables de tiempo
unsigned long tiempo1 = 0;
unsigned long tiempo2 = 0;

//variables de diferencias de tiempo
unsigned long diferenciaTiempo=0;

//variables de lectura de botones
int Lboton1=0;
int Lboton2=0;
int sumabotones=0;
int lectura_de_interruptor;

// variable para TELEGRAM
char mensaje;

//constantes de RED
const char* ssid = "Nombre de mi red";
const char* password = "Contraseña de mi red";

//tokens para CHATBOTS
#define BOTtoken "5513596431:AAGQm7NEc0qF976_0z2-DTrUxPZuBH1AMp4"  // your Bot Token (Get from Botfather)
#define CHAT_ID "1299899272"

#define BOTtoken1 "5386748185:AAEEpAeT7E6_iEmw_55Z7Vf2BjuO4-GUGSk"
#define CHAT_ID1 "1299899272"

X509List cert(TELEGRAM_CERTIFICATE_ROOT);
WiFiClientSecure client;

UniversalTelegramBot bot(BOTtoken, client);
UniversalTelegramBot bot1(BOTtoken1, client);

void setup() {

  //velocidad del SERIAL
  Serial.begin(115200);
  //Modo de operacion de los PINES
  pinMode(interruptor, INPUT);
  pinMode(boton1, INPUT);
  pinMode(boton2, INPUT);

  //funcion millis
  tiempo1=millis(); //tiempo para sensor 1 
  
  // conexion WiFi
  Serial.print("Connecting Wifi: ");
  Serial.println(ssid);

  WiFi.mode(WIFI_STA);
  WiFi.begin(ssid, password);

  //si no se encuentra conectado
  while (WiFi.status() != WL_CONNECTED) {
    Serial.print(".");
    delay(500);
  }
  //si se encuentra conectado
  Serial.println("Dispositivo conectado");
  Serial.println("direccion IP:");
  Serial.println(WiFi.localIP());
}

void loop() {

     //lectura de sensores
     Lboton1=digitalRead(boton1);
     Lboton2=digitalRead(boton2);
     lectura_de_interruptor=digitalRead(interruptor);
     sumabotones=Lboton1+Lboton2;

     //Rutina
     //si el sensor 1 detecta estado alto
     if(Lboton1==1){
 
              tiempo2=millis(); //inicia tiempo2
              diferenciaTiempo=tiempo2/1000 - tiempo1/1000;
              Serial.println(diferenciaTiempo);

     if(diferenciaTiempo==20){//si han pasado 20 min
              
             Serial.println(" han pasado 20min de inactividad "); 
             bot.sendMessage(CHAT_ID, "Alarma 1");//mandar mensaje a chatbot 1
     }
     if(diferenciaTiempo==40){ //si han pasado 40 min
              Serial.println("2 han pasado 40min de inactividad ");
              bot1.sendMessage(CHAT_ID1, "Alarma 2");//mandar mensaje a chatbot 2
     }
     
    if(lectura_de_interruptor==1){ //hora de descanso
          
             bot.sendMessage(CHAT_ID,   "Descanso/Hora de comida");//mandar mensaje a chatbot 1
             bot1.sendMessage(CHAT_ID1, "Descanso/Hora de comida");//mandar mensaje a chatbot 2
             delay(3000);
 

}
}
else{}
}

```

