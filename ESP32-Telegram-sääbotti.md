# Telegram-sääbotti ESP32-mikrokontrollerilla  
  
## 1 Johdanto  
  
Tämä seminaarityö perustuu pohjimmiltaan käyttäjän tarpeeseen:  
  
*"Kuinka voin saada älypuhelimeeni reaaliaikaista tietoa ilmanpaineesta sekä lämpötilasta?"*   
  
Työn tarkoituksena on toteuttaa yksi mahdollinen ratkaisu tarpeen täyttämiseksi. Ratkaisu toteutetaan hyödyntäen ESP32-mikrokontrolleria, joka ohjelmoidaan Arduino-kehitysympäristöllä lähettämään säätietoja Telegram-botille.  
  
Tavoitteena on, että käyttäjä pystyy eri komennoilla vastaanottamaan Telegram-botilta tietoa ilmanpaineesta, sekä lämpötilasta.  
  
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
  
Arduino IDE (Integrated Development Environment) on ohjelmisto, jota voidaan käyttää koodin kirjoittamiseen, kääntämiseen ja lataamiseen Arduino-laitteisiin. Ohjelmointikielenä toimii C++ ohjelmointikieleen pohjautuva kieli.  
  
### 3.1 Kehitysympäristön asennus  
  
Tässä työssä käytetään kehitysympäristönä Arduino IDE-ohjelmiston versiota 1.18.9.  
  
Kehitysympäristön voi ladata osoitteesta https://www.arduino.cc/en/software  
  
Valitaan *Arduino IDE 1.8.19 -> Windows app*  
  
  
![image](https://user-images.githubusercontent.com/90974678/235227144-0453233e-1b8f-4b97-b0f0-8f89605626b2.png)
  
Kun Arduino IDE on asennettu, sen pitäisi avautua suoraan uuteen tiedostoon.  
  
![image](https://user-images.githubusercontent.com/90974678/235234494-eece11b8-198e-483e-9c62-d88f32be5e22.png)


