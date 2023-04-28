# Telegram-sääbotti ESP32-mikrokontrollerilla  
 
## 1 Johdanto  
  
Tämä seminaarityö perustuu pohjimmiltaan käyttäjän tarpeeseen:  
  
*"Kuinka voin saada älypuhelimeeni reaaliaikaista tietoa ilmanpaineesta sekä lämpötilasta?"*   
  
Työn tarkoituksena on toteuttaa yksi mahdollinen ratkaisu tarpeen täyttämiseksi. Ratkaisu toteutetaan hyödyntäen ESP32-mikrokontrolleria, joka ohjelmoidaan Arduino-kehitysympäristöllä lähettämään säätietoja Telegram-botille.  
  
Tavoitteena on, että käyttäjä pystyy eri komennoilla vastaanottamaan Telegram-botilta tietoa ilmanpaineesta, sekä lämpötilasta.  
  
### Seminaarityössä käytettävät Tutoriaalit  

Seminaarityössä toteutettava ohjelma perustuu artikkeliin
  
Telegram: Control ESP32/ESP8266 Outputs (Arduino IDE). RandomNerdTutorials.com. Luettavissa: https://randomnerdtutorials.com/telegram-control-esp32-esp8266-nodemcu-outputs/  
  
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
  
## 5 Telegram-botin luominen  
  
Seuraavaksi tarvitsemme uuden Telegram-botin, jonka voimme yhdistää ohjelmaan.  
  
Telegram-botteja on mahdollista luoda käyttäen Telegramista valmiiksi löytyvää *BotFather*-bottia. 
  
![image](https://user-images.githubusercontent.com/90974678/235255373-b6622175-8095-47aa-9a38-688470402d3a.png)
  
Uuden botin luominen onnistuu ensin käynnistämällä BotFather komennolla ```/start``` ja sitten antamalla komento ```/newbot```  
  
BotFather pyytää antamaan botille nimen. Tässä tapauksessa annetaan botille vaikkapa nimi *WeatherBot*  
  
Lisäksi botille tarvitsee antaa käyttäjänimi, jonka täytyy päättyä sanaan *bot*. Tässä tapauksessa annetaan botille vaikkapa nimi *WeatherPressure_bot*. Käyttäjänimen täytyy olla uniikki, joten esimerkiksi tälle botille annettu käyttäjänimi ei ole enää saatavilla.  
  
![image](https://user-images.githubusercontent.com/90974678/235256221-5bac9d32-cf0c-4a50-9c61-af15c76b2fcd.png)
  
BotFather luo uuden botin. Botille luodaan *token*, jolla bottiin voidaan ottaa yhteys koodissa. Token on henkilökohtainen ja väärinkäytösten estämiseksi sitä ei saa jakaa julkisesti muille.  
  
Botti on valmis käytettäväksi.  

  



