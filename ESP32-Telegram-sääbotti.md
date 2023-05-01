# Telegram-sääbotti ESP32-mikrokontrollerilla  
 
## 1 Johdanto  
  
Tämä seminaarityö perustuu pohjimmiltaan käyttäjän tarpeeseen:  
  
*"Kuinka voin saada älypuhelimeeni reaaliaikaista tietoa ilmanpaineesta sekä lämpötilasta?"*   
  
Työn tarkoituksena on toteuttaa yksi mahdollinen ratkaisu tarpeen täyttämiseksi. Ratkaisu toteutetaan hyödyntäen ESP32-mikrokontrolleria, joka ohjelmoidaan Arduino-kehitysympäristöllä lähettämään säätietoja Telegram-botille.  
  
Tavoitteena on, että käyttäjä pystyy eri komennoilla vastaanottamaan Telegram-botilta tietoa ilmanpaineesta, sekä lämpötilasta.  
  
### Seminaarityössä käytettävät Tutoriaalit  

Seminaarityössä toteutettava ohjelma perustuu artikkeliin
  
Telegram: Request ESP32/ESP8266 Sensor Readings (Arduino IDE). RandomNerdTutorials.com. Luettavissa: https://randomnerdtutorials.com/telegram-request-esp32-esp8266-nodemcu-sensor-readings/
  
Arduino IDE-kehitysympäristön / ESP32-kehitysalustan asennusohjeet perustuvat artikkeliin  
  
Installing ESP32 Board in Arduino IDE 2.0 (Windows, Mac OS X, Linux). RandomNerdTutorials.com. Luettavissa: https://randomnerdtutorials.com/installing-esp32-arduino-ide-2-0/#:~:text=To%20install%20the%20ESP32%20board%20in%20your%20Arduino,the%20install%20button%20for%20esp32%20by%20Espressif%20Systems.  
  

## 2 Käytettävät teknologiat  
  
Työ toteutetaan käyttäen seuraavia välineitä:  
  
### 2.1 Rauta  
  
* ESP32-mikrokontrolleri  
  
![image](https://user-images.githubusercontent.com/90974678/235227486-922258a0-9d7e-406e-9528-0a4b1db129ee.png)
  
  
* BMP280 ilmanpaine/lämpötila-sensori  
  
![image](https://user-images.githubusercontent.com/90974678/235227393-d79af9fc-607f-41e7-aa45-0c386da9636f.png)  
  
* Koekytkentälevy  

* X Uros-Uros hyppylanka
* X Uros-Naaras hyppylanka
  
### 2.2 Ohjelmisto
  
* Arduino IDE 1.8.19  
* Telegram BotFather  

## 3 Arduino IDE  
  
Arduino IDE (Integrated Development Environment) on ohjelmisto, jota voidaan käyttää koodin kirjoittamiseen, kääntämiseen ja lataamiseen Arduino-kehitysalustalle. Ohjelmointikielenä toimii C++ ohjelmointikieleen pohjautuva kieli.  
  
### 3.1 Kehitysympäristön asennus  
  
Tässä työssä käytetään kehitysympäristönä Arduino IDE-ohjelmiston versiota 1.18.9.  
  
Kehitysympäristön voi ladata osoitteesta https://www.arduino.cc/en/software  
  
Valitaan *Arduino IDE 1.8.19 -> Windows app*  
  
  
![image](https://user-images.githubusercontent.com/90974678/235227144-0453233e-1b8f-4b97-b0f0-8f89605626b2.png)  
  
### 3.2 Kehitysympäristön perusteet   
  
Kun Arduino IDE on asennettu, sen pitäisi avautua suoraan uuteen tiedostoon.  
  
![image](https://user-images.githubusercontent.com/90974678/235234494-eece11b8-198e-483e-9c62-d88f32be5e22.png)
  
Arduinossa kirjoitettavia ohjelmia kutsutaan nimellä *Sketch*. Ikkunan yläreunasta voidaan lukea, että tällä hetkellä auki oleva ohjelma on nimeltään ```sketch_apr28a```.  
  
Nimen loppuosasta ```apr28``` voidaan lukea, että kyseinen ohjelma on luotu 28.4. ja ```a``` kertoo kyseessä olevan ensimmäinen sinä päivänä luotu ohjelma.  
  
Lisäksi Arduino IDE:stä löytyy esimerkiksi *Serial Monitor*, jota voidaan käyttää muun muassa ohjelman debuggaukseen sekä kehitysalustan ja tietokoneen välisen kommunikoinnin seuraamiseen.  
  
![image](https://user-images.githubusercontent.com/90974678/235237097-fdaefd0b-f7bb-43ef-8773-0a889b3f075b.png)  
  
### 3.3 ESP32-tuen asennus kehitysympäristöön  
  
Jotta voimme toteuttaa ohjelmia ESP32-mikrokontrolleria käyttäen, tarvitsee ensin asentaa Arduino IDE:en ESP32-tuki.  
  
Siirrytään ikkunan yläreunasta *File -> Preferences* 
  
![image](https://user-images.githubusercontent.com/90974678/235237783-dd80757d-19ed-4b90-80f0-b39a18f01e69.png)  
  
Lisätään kohtaan ```Additional Board Manager URL:s``` seuraava linkki:  
  
https://raw.githubusercontent.com/espressif/arduino-esp32/gh-pages/package_esp32_index.json  
  
![image](https://user-images.githubusercontent.com/90974678/235238443-fa15f41a-2b42-4abc-8b85-b16325b8c531.png)
  
Jonka jälkeen OK.  
  
Siirrytään ikkunan yläreunasta *Tools -> Boards Manager*  
  
![image](https://user-images.githubusercontent.com/90974678/235241903-1e4bf809-f7d1-4d22-ab4c-cd12829ff18e.png)  
  
Kirjoitetaan hakukenttään *esp32* ja asennetaan.  
  
![image](https://user-images.githubusercontent.com/90974678/235242551-4a60ba9c-84e7-4f97-8306-be71f48ef6fc.png)
  
Nyt kehitysympäristöön on asennettu tuki ESP32-mikrokontrollerille ja on mahdollista alkaa toteuttamaan ohjelmaa.  
  
## 4 Telegram-botin luominen  
  
### 4.1 BotFather
  
Seuraavaksi tarvitsemme uuden Telegram-botin, jonka voimme yhdistää ohjelmaan.  
  
Telegram-botteja on mahdollista luoda käyttäen Telegramista valmiiksi löytyvää *BotFather*-bottia. 
  
![image](https://user-images.githubusercontent.com/90974678/235255373-b6622175-8095-47aa-9a38-688470402d3a.png)
  
Uuden botin luominen onnistuu käynnistämällä BotFather komennolla ```/start``` ja sitten antamalla komento ```/newbot```  
  
BotFather pyytää antamaan botille nimen. Tässä tapauksessa annetaan botille vaikkapa nimi *WeatherBot*  
  
Lisäksi botille tarvitsee antaa käyttäjänimi, jonka täytyy päättyä sanaan *bot*. Tässä tapauksessa annetaan botille vaikkapa nimi *WeatherPressure_bot*. Käyttäjänimen täytyy olla uniikki, joten esimerkiksi tälle botille annettu käyttäjänimi ei ole enää saatavilla.  
  
![image](https://user-images.githubusercontent.com/90974678/235256221-5bac9d32-cf0c-4a50-9c61-af15c76b2fcd.png)
  
BotFather luo uuden botin. Botille luodaan *token*, jolla bottiin voidaan ottaa yhteys koodissa. Token on henkilökohtainen ja väärinkäytösten estämiseksi sitä ei saa jakaa julkisesti muille.  
  
Botti on valmis käytettäväksi.  
  
### 4.2 IDBot  
  
Telegram-botin käyttöoikeudet voidaan rajata vain tietylle henkilölle käyttäen henkilökohtaista käyttäjä-ID:tä. Jos käyttäjä-ID:tä ei aseteta koodiin, kuka tahansa voi käyttää bottia vapaasti.  
  
Oman Telegram käyttäjä-ID:n voi löytää helposti käyttäen IDBot-bottia.  
  
![image](https://user-images.githubusercontent.com/90974678/235512293-0757b2c3-dbfe-41c0-86ef-0552e01a7461.png)
  
![image](https://user-images.githubusercontent.com/90974678/235512442-5ac8722c-13e0-4e96-8b89-84e6c8047b68.png)
  
Käyttäjä-ID on luonnollisesti henkilökohtainen ja sitä ei tule jakaa muille käyttäjille.  
  
## 5 Ohjelman toteuttaminen  
  
Tässä seminaarityössä ei opeteta kohta kohdalta miten ohjelmoidaan itse ohjelma, joka kommunikoi ESP32-mikrokontrollerin ja Telegramin kanssa. Sen sijaan työssä käytetään valmista RandomNerdTutorials.com sivustolta löytyvää koodia, jota on muokattu niin, että botti toimii halutulla tavalla ja esitellään koodin oleellisia kohtia.  
  
### 5.1 Tarvittavat kirjastot  
  
Ohjelman toimimiseksi tarvitsee ensin asentaa muutama kirjasto. Kirjastoja pystyy Arduino IDE:ssä asentamaan siirtymällä yläpalkista *Tools -> Manage libraries*, jolloin aukeaa *Library Manager*  
  
![image](https://user-images.githubusercontent.com/90974678/235513701-91c51416-a162-41d6-9c1a-b6016912ba83.png)
  
Kirjastojen asentaminen onnistuu etsimällä haluttua kirjastoa hakupalkista ja klikkaamalla *Install*.  
  
#### Asennettavat kirjastot  
  
* UniversalTelegramBot
  
![image](https://user-images.githubusercontent.com/90974678/235514222-438ce294-04eb-463b-b6b7-d58d8950783e.png)
  
* ArduinoJson  
  
![image](https://user-images.githubusercontent.com/90974678/235514355-ca5f184b-ead3-426a-991e-dbd1a9fd0765.png)
  
* Adafruit_BMP280
  
![image](https://user-images.githubusercontent.com/90974678/235514429-baf32bf7-3e62-408b-be47-15a0632556fd.png)
  
* Adafruit_Sensor  
  
![image](https://user-images.githubusercontent.com/90974678/235514638-930c6220-8b37-4481-92ff-3cfa3dd9cf41.png)

### 5.2 Koodi
  
Toimiva ohjelma näyttää seuraavalta:
  
```
/*
  Rui Santos
  Complete project details at https://RandomNerdTutorials.com/telegram-request-esp32-esp8266-nodemcu-sensor-readings/
  
  Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files.
  The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

  Project created using Brian Lough's Universal Telegram Bot Library: https://github.com/witnessmenow/Universal-Arduino-Telegram-Bot
*/

#ifdef ESP32
  #include <WiFi.h>
#else
  #include <ESP8266WiFi.h>
#endif
#include <WiFiClientSecure.h>
#include <UniversalTelegramBot.h> // Universal Telegram Bot Library written by Brian Lough: https://github.com/witnessmenow/Universal-Arduino-Telegram-Bot
#include <ArduinoJson.h>
#include <Adafruit_BMP280.h>
#include <Adafruit_Sensor.h>

// Replace with your network credentials
const char* ssid = "xxxxxxxxxx";
const char* password = "xxxxxxxxxx";

// Use @myidbot to find out the chat ID of an individual or a group
// Also note that you need to click "start" on a bot before it can
// message you
#define CHAT_ID "xxxxxxxxx"

// Initialize Telegram BOT
#define BOTtoken "xxxxxxxxxx:xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx-xxx"  // your Bot Token (Get from Botfather)

#ifdef ESP8266
  X509List cert(TELEGRAM_CERTIFICATE_ROOT);
#endif

WiFiClientSecure client;
UniversalTelegramBot bot(BOTtoken, client);

//Checks for new messages every 1 second.
int botRequestDelay = 1000;
unsigned long lastTimeBotRan;

// BMP280 connect to ESP32 I2C (GPIO 21 = SDA, GPIO 22 = SCL)
// BMP280 connect to ESP8266 I2C (GPIO 4 = SDA, GPIO 5 = SCL)
Adafruit_BMP280 bmp; // I2C Interface       

// Get BMP280 sensor readings and return them as a String variable
String getPresTemp(){
  float temperature, pressure;
  temperature = bmp.readTemperature();
  pressure = bmp.readPressure();
  String message = "Temperature: " + String(temperature) + " ºC \n";
  message += "Pressure: " + String (pressure / 100 ) + " % \n";
  return message;
}

String getTemperature(){
  float temperature;
  temperature = bmp.readTemperature();
  String message = "Temperature: " + String(temperature) + " ºC \n";
  return message;
}

String getPressure(){
  float pressure;
  pressure = bmp.readPressure();
  String message = "Pressure: " + String (pressure / 100) + " % \n";
  return message;
}

//Handle what happens when you receive new messages
void handleNewMessages(int numNewMessages) {
  Serial.println("handleNewMessages");
  Serial.println(String(numNewMessages));

  for (int i=0; i<numNewMessages; i++) {
    // Chat id of the requester
    String chat_id = String(bot.messages[i].chat_id);
    if (chat_id != CHAT_ID){
      bot.sendMessage(chat_id, "Unauthorized user", "");
      continue;
    }
    
    // Print the received message
    String text = bot.messages[i].text;
    Serial.println(text);

    String from_name = bot.messages[i].from_name;

    if (text == "/start") {
      String welcome = "Welcome, " + from_name + ".\n";
      welcome += "Use the following command to get current readings.\n\n";
      welcome += "/temperature \n";
      welcome += "/pressure \n";
      welcome += "/prestemp \n";
      bot.sendMessage(chat_id, welcome, "");
    }

    if (text == "/prestemp") {
      String readings = getPresTemp();
      bot.sendMessage(chat_id, readings, "");
    }  

    else if (text == "/temperature") {
      String temperature = getTemperature();
      bot.sendMessage(chat_id, temperature, "");
    }

    else if (text == "/pressure") {
      String pressure = getPressure();
      bot.sendMessage(chat_id, pressure, "");
    }
  }
}

void setup() {
  Serial.begin(115200);

  #ifdef ESP8266
    configTime(0, 0, "pool.ntp.org");      // get UTC time via NTP
    client.setTrustAnchors(&cert); // Add root certificate for api.telegram.org
  #endif
  
  // Init BMP280 sensor
  if (!bmp.begin(0x76)) {
    Serial.println("Could not find a valid BME280 sensor, check wiring!");
    while (1);
  }
  
  // Connect to Wi-Fi
  WiFi.mode(WIFI_STA);
  WiFi.begin(ssid, password);
  #ifdef ESP32
    client.setCACert(TELEGRAM_CERTIFICATE_ROOT); // Add root certificate for api.telegram.org
  #endif
  while (WiFi.status() != WL_CONNECTED) {
    delay(1000);
    Serial.println("Connecting to WiFi..");
  }
  // Print ESP32 Local IP Address
  Serial.println(WiFi.localIP());
}

void loop() {
  if (millis() > lastTimeBotRan + botRequestDelay)  {
    int numNewMessages = bot.getUpdates(bot.last_message_received + 1);

    while(numNewMessages) {
      Serial.println("got response");
      handleNewMessages(numNewMessages);
      numNewMessages = bot.getUpdates(bot.last_message_received + 1);
    }
    lastTimeBotRan = millis();
  }
}
```  
  
### 5.3 Tärkeitä kohtia koodissa  
  
#### Verkkoyhteys 
  
ESP32-tarvitsee tietojen lähettämistä varten verkkoyhteyden, joten koodiin tarvitsee asettaa toimivan tukiaseman tiedot.  

``` 
// Replace with your network credentials
const char* ssid = "xxxxxxxxxx";
const char* password = "xxxxxxxxxx";  
```  
  
#### Chat-ID ja BotToken  
  
Aiemmin hankitut BotToken ja Chat-ID tulee myös asettaa koodiin.  
  
```
#define CHAT_ID "xxxxxxxxx"

// Initialize Telegram BOT
#define BOTtoken "xxxxxxxxxx:xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx-xxx"  // your Bot Token (Get from Botfather)
```  
On myös tärkeää huolehtia, etteivät nämä henkilökohtaiset tiedot päädy julkisesti näkyville esimerkiksi Git-versionhallinnassa.  
  

  


  


