# Telegram-sääbotti ESP32-mikrokontrollerilla  
  
## Sisällysluettelo
  * [1 Johdanto](#1-johdanto)
    + [Seminaarityössä käytettävät Tutoriaalit](#seminaarityössä-käytettävät-tutoriaalit)
  * [2 Käytettävät teknologiat](#2-käytettävät-teknologiat)
    + [2.1 Rauta](#21-rauta)
    + [2.2 Ohjelmistot](#22-ohjelmistot)
  * [3 Arduino IDE](#3-arduino-ide)
    + [3.1 Kehitysympäristön asennus](#31-kehitysympäristön-asennus)
    + [3.2 Kehitysympäristön perusteet](#32-kehitysympäristön-perusteet)
    + [3.3 ESP32-tuen asennus kehitysympäristöön](#33-esp32-tuen-asennus-kehitysympäristöön)
  * [4 Telegram-botin luominen](#4-telegram-botin-luominen)
    + [4.1 BotFather](#41-botfather)
    + [4.2 IDBot](#42-idbot)
  * [5 Ohjelman toteuttaminen](#5-ohjelman-toteuttaminen)
    + [5.1 Kirjastot](#51-kirjastot)
    + [5.2 Koodi](#52-koodi)
    + [5.3 Tärkeitä kohtia koodissa](#53-tärkeitä-kohtia-koodissa)
  * [6 Härveli / Vempele / Laite](#6-härveli--vempele--laite)
    + [6.1 Komponentteja ja Merkintöjä](#61-komponentteja-ja-merkintöjä)
    + [6.2 Kokoaminen](#62-kokoaminen)
  * [7 Koodin lataaminen ESP32-mikrokontrolleriin ja botin käynnistys](#7-koodin-lataaminen-esp32-mikrokontrolleriin-ja-botin-käynnistys)
    + [7.1 Koodin lataaminen](#71-koodin-lataaminen)
    + [7.2 Telegram-botin käynnistys](#72-telegram-botin-käynnistys)
  * [8 Yhteenveto](#8-yhteenveto)
  * [9 Video](#9-video)
 
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
  
![IMG_20230501_224415](https://user-images.githubusercontent.com/90974678/235737175-3e6f76bd-0689-4191-93d2-a87e5f150fda.jpg)


* 4 kpl Uros-Uros hyppylankaa
* 4 kpl Uros-Naaras hyppylanka
  
### 2.2 Ohjelmistot
  
* Arduino IDE 1.8.19  
* Telegram BotFather
* Telegram IDBot  

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
  
Botti on valmis käytettäväksi koodissa.  
  
### 4.2 IDBot  
  
Telegram-botin käyttöoikeudet voidaan rajata vain tietylle henkilölle käyttäen henkilökohtaista käyttäjä-ID:tä. Jos käyttäjä-ID:tä ei aseteta koodiin, kuka tahansa voi käyttää bottia vapaasti.  
  
Oman Telegram käyttäjä-ID:n voi löytää helposti käyttäen IDBot-bottia.  
  
![image](https://user-images.githubusercontent.com/90974678/235512293-0757b2c3-dbfe-41c0-86ef-0552e01a7461.png)
  
![image](https://user-images.githubusercontent.com/90974678/235512442-5ac8722c-13e0-4e96-8b89-84e6c8047b68.png)
  
Käyttäjä-ID on luonnollisesti henkilökohtainen ja sitä ei tule jakaa muille käyttäjille.  
  
## 5 Ohjelman toteuttaminen  
  
Tässä seminaarityössä ei opeteta kohta kohdalta miten ohjelmoidaan itse ohjelma, joka kommunikoi ESP32-mikrokontrollerin ja Telegramin kanssa. Sen sijaan työssä käytetään valmista RandomNerdTutorials.com sivustolta löytyvää koodia, jota on muokattu niin, että botti toimii halutulla tavalla ja esitellään koodin oleellisia kohtia.  
  
### 5.1 Kirjastot  
  
Ohjelman toimimiseksi tarvitsee ensin asentaa muutama kirjasto. Kirjastoja pystyy Arduino IDE:ssä asentamaan siirtymällä yläpalkista *Tools -> Manage libraries*, jolloin aukeaa *Library Manager*  
  
![image](https://user-images.githubusercontent.com/90974678/235513701-91c51416-a162-41d6-9c1a-b6016912ba83.png)
  
Kirjastojen asentaminen onnistuu etsimällä haluttua kirjastoa hakupalkista ja klikkaamalla *Install*.  
  
#### Kirjastot  
  
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
  
ESP32-tarvitsee tietojen lähettämistä varten verkkoyhteyden, joten koodiin tulee asettaa toimivan tukiaseman tiedot.  

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
  
## 6 Härveli / Vempele / Laite 
  
Jotta toteutettu ohjelma voidaan saada toimivaksi, tarvitaan laite, johon ohjelma ladataan. 
  
Käydään lyhyesti läpi, miten tarvittava laite voidaan koota.  
  
### 6.1 Komponentteja ja Merkintöjä  
  
#### Koekytkentälevy  
  
Koekytkentälevy, tai *"Breadboard"* on levy, jolla voidaan rakentaa ja testata erilaisia elektronisia piirejä.  
  
Koekytkentälevy on jaettu kahteen puoleen. Kummallakin puolella on rivejä, joiden reikiin voidaan asettaa komponentteja ilman juottamista ja reikiä yhdistävät useat metalliset johtimet, jotka mahdollistavat erilaisten kytkentöjen tekemisen. 

![image](https://user-images.githubusercontent.com/90974678/235748811-70a7ca4b-2dbb-4b83-961c-238d4a8f835b.png)
  
(*kuva: Modified the breadboard created by https://openclipart.org/user-detail/oscillator)*
  
  
#### BMP280  
  
BMP280 on ilmanpaine- ja lämpötilasensori jota voidaan käyttää yhdessä ESP32-mikrokontrollerin kanssa. 
  
BMP280-sensorista löytyy pinnejä, joiden merkinnät kertovat mitkä johdot halutaan kytkeä mihinkin.  
  
##### VCC / GND
  
VCC ja GND ovat virtapinnejä. Lyhyesti, VCC yhdistetään virtalähteen positiiviseen (+) napaan ja sillä saadaan sensoriin virtaa, kun taas GND tarkoittaa maadoitusta ja se yhdistetään negatiiviseen (-) napaan.  
  
##### SDA / SCL  
  
SDA ja SCL ovat pinnejä, jotka käyttävät I2C-protokollaa tiedonsiirtoon laitteiden välillä. SDA on lyhenne sanoista "Serial Data" ja sen tarkoitus on siirtää tietoa, kun taas SCL on lyhenne sanoista "Serial Clock" ja sen tarkoitus on synkronoida tiedonsiirto laitteiden välillä.  
  
  
  
Lisäksi sensorista löytyy CSB (Chip Select Bar) ja SDO (Serial Data Out) -pinnit, joita ei kuitenkaan tässä työssä tarvita.  
  

![IMG_20230501_224430](https://user-images.githubusercontent.com/90974678/235737324-14192f08-2983-4cfd-891e-133d51445104.jpg)
  
  
#### ESP32  
  
ESP32 on *Espressif Systemsin* kehittämä mikrokontrolleri, jota voidaan käyttää esimerkiksi IoT (Internet of Things)-laitteiden kehityksessä.  
  
Mikrokontrollerista löytyy useita erilaisia pinnejä, joiden merkinnät niin ikään auttavat haluttujen kytkentöjen tekemisessä.  
 
##### 3V3  
  
ESP32-mikrokontrollerista löytyy sekä *3.3*, että *5* voltin virtalähdöt. BMP280-sensori tarvitsee 3.3 voltin virran, joten tässä työssä käytetään sitä. 3v3-pinnistä kytketään johto koekytkentälevyyn, positiiviseen (+) napaan.  
  
##### GND  
  
ESP32:sta löytyy useampi GND-pinni, jotka tarkoittavat maadoitusta. Maadoituksesta kytketään johto koekytkentälevyyn, negatiiviseen (+) napaan samalle puolelle 3v3-johdon kanssa.  
  
##### G21 / G22  
  
G21 ja G22 ovat pinnejä, joita käytetään I2C-tiedonsiirtoon. Nämä pinnit toimivat siis yhdessä BMP280-sensorin kanssa. 
  
*G21* vastaa SDA-linjaa ja *G22* SCL-linjaa, joten ne halutaan yhdistää vierekkäin BMP280-sensorin vastaavien SDA ja SCL-linjojen kanssa.  
  
![image](https://user-images.githubusercontent.com/90974678/235737594-fc8e7fdb-8011-49c9-b998-e92b629b31a2.png)
  
  
### 6.2 Kokoaminen  
  
Aloitetaan laitteen kokoaminen antamalla BMP280-sensorille virtaa.  
  
Kytketään ESP32-mikrokontrollerin *3v3*-pinni johdolla kiinni koekytkentälevyn positiiviseen napaan ja *GND*-pinni negatiiviseen napaan sen viereen. Ei ole oikeastaan väliä, vaikka johdot eivät olisi koekytkentälevyssä vierekkäin, mutta se on siistimpää ja selkeämpää.

![image](https://user-images.githubusercontent.com/90974678/235737723-09380622-2ca2-4d28-ac35-c564f993f669.png)
*(Punainen johto: 3v3, Valkoinen johto: GND)*
  
Seuraavaksi kytketään BMP280-sensorin VCC-pinni saman puolen virtakiskojen positiiviseen- ja GND-pinni negatiiviseen napaan.  
  
![image](https://user-images.githubusercontent.com/90974678/235737783-f6147b80-0aa6-4fe9-8d1c-0f56c7ecea8e.png)  
*(Musta johto: VCC, Valkoinen johto: GND)*  
  
Kun BMP280-sensori saa virtaa, halutaan kytkeä sensorin SDA-pinni koekytkentälevyn reikään riville 21 ja SCL-pinni riville 22.  
  
![image](https://user-images.githubusercontent.com/90974678/235737836-cb03f54a-a76a-4258-9758-2bb7181da21c.png)  
*(Vihreä johto: SCL, Musta johto: SDA)*
  
Lopuksi halutaan vielä kytkeä ESP32-mikrokontrollerista pinnit G21 ja G22 oikeille paikoilleen koekytkentälevyyn BMP280-sensorin SDA ja SCL-pinnien viereen.  
  
![image](https://user-images.githubusercontent.com/90974678/235738127-c05f8877-b385-4ad7-bd2c-27f63ceacafd.png)  
*(Ruskeat johdot G21 SDA-johdon viereen ja G22 SCL-johdon viereen)*  
  
Kun laite on koottu, se yhdistetään ESP32:sta löytyvästä portista *Micro-USB*-johdolla tietokoneeseen.  
  
![image](https://user-images.githubusercontent.com/90974678/235754144-c07da56f-138a-4c9e-ae6d-e74d3613a8a1.png)  
  
ESP32-mikrokontrolleriin pitäisi tietokoneeseen yhdistäessä syttyä pieni LED-valo merkiksi siitä, että se saa virtaa.  
  
## 7 Koodin lataaminen ESP32-mikrokontrolleriin ja botin käynnistys  
  
### 7.1 Koodin lataaminen  
  
Nyt kun koottu laite on yhdistetty tietokoneeseen, on aika ladata sille koodi.  
  
Koodin lataaminen onnistuu Arduino IDE:ssä klikkaamalla yläreunasta *Upload*-painiketta.  

![image](https://user-images.githubusercontent.com/90974678/235755783-a588d101-1c09-4333-9b4d-0cec7dc22c3c.png)
  
Ikkunan alareunaan ilmestyy latauspalkki. Koodin latauksessa saattaa kestää jonkin aikaa.  
  
![image](https://user-images.githubusercontent.com/90974678/235756384-8feef239-e4c0-4f8a-8066-0c112a40e6b5.png)  
  
Kun koodi on ladattu laitteeseen, laite käynnistyy ja alkaa keräämään tietoa lämpötilasta sekä ilmanpaineesta.

### 7.2 Telegram-botin käynnistys  
  
Nyt kun laite kerää säätietoja, voimme pyytää niitä aiemmin luodulta Telegram-botilta. 

![image](https://user-images.githubusercontent.com/90974678/235757478-01fd4e90-5ec4-439d-bf20-9053b57345d6.png)  
  
Käynnistetään botti komennolla ```/start```.  
  
Botti käynnistyy ja kaikeksi iloksi on valmiina antamaan reaaliaikaista tietoa ilmanpaineesta ja lämpötilasta!  
  
![image](https://user-images.githubusercontent.com/90974678/235757922-44709168-9a10-489c-a7ce-8d5fd0154171.png)  
  
Testataan toimintaa vielä älypuhelimella  
  
![Screenshot_2023-05-02-21-52-29-39_948cd9899890cbd5c2798760b2b95377](https://user-images.githubusercontent.com/90974678/235758917-a15aed7a-bc52-4c99-b93f-51152c76c280.jpg)

  
## 8 Yhteenveto  
  
Tämän seminaarityön tavoitteena oli esitellä yksi mahdollinen ratkaisu käyttäjän tarpeeseen saada älypuhelimeen reaaliaikaista tietoa ilmanpaineesta ja lämpötilasta. Työ toteutettiin hyödyntäen ESP32-mikrokontrolleria, joka ohjelmoitiin lähettämään Telegram-botille tietoja, jotka se vastaanottaa BMP280 ilmanpaine ja lämpötila-sensorilta.  
  
Seminaarityö auttoi minua syventämään osaamistani teknologioista, joiden kanssa työskentelin osallistuessani International IT-seminar kurssille keväällä 2023. Opin kuinka erilaisilla sensoreilla kuten BMP280 voidaan kerätä tietoa ja kuinka esittää kerättyä tietoa Internetissä. Lisäksi syvensin osaamistani Telegram-bottien toteuttamisessa. Kiinnostuin International IT-seminar kurssilla IoT-laitteista ja uskon, että tämä seminaarityö auttaa minua tulevaisuudessa jatkamaan haastavampien IoT-laitteiden toteuttamista.  
  
## 9 Video  
  
Linkki videoon seminaarityön teknisestä toteutuksesta: https://www.youtube.com/watch?v=PneckDcxS24





