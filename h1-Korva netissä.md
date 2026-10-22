## x) Spektrini. 

<img width="1778" height="1086" alt="image" src="https://github.com/user-attachments/assets/e7d7a5a3-4842-4b8f-aa6b-8b78b06025ac" />

 - taajuus kasvaa oikealle, aallonpituus pienenee samaan suuntaan (c = f·λ)
 - siksi korkealla taajuudella antenni on pieni ja dataa mahtuu enemmän, matalalla taajuudella signaali taas läpäisee seiniä ja kantaa kauemmas


## a) WebSDR

Aloitin tehtävän menemällä WebSDR sivulle ja valitsin ensimmäisen vastaanottimen, joka oli Twenten yliopiston (Alankomaat) radio kerhon rakentama ja ylläpitämä radiovastaanotin. Valitsin suodattimesta AM ja sitten etsin 31 metrin kaistalta vesiputouksesta kirkasta pystyviivaa ja viritin siihen.

<img width="1798" height="1522" alt="image" src="https://github.com/user-attachments/assets/a3a722d4-bc62-4c4f-a977-326cbc726879" />

<img width="1802" height="1198" alt="image" src="https://github.com/user-attachments/assets/21bf82b8-a21e-4c6f-8e30-8bce32fa81bb" />

Päädyin Kiinan yhteen yleisradioon (taajuus 9600 kHz), jossa kuului hieman heikosti puhetta ja jotain musiikkia. Vesiputous näytti kanavan kohdalta kirkkaammalta eli se näyttää, että jotain tietoa/dataa tulee siltä taajuudelta. Kun mietin lopuksi vielä aallonpituutta niin se sai laskettua kaavalla λ = 300 / f(MHz) = 300 / 9,6 ≈ 31,3 m. Tämä vastasi kaistan nimeä `31 m broadcast`.

## b) Moduwhat?

Pysyin samassa Twenten vastaanottimessa, viritin taajuudelle 14208 kHz ja valitsin modulaatioksi USB. Sieltä kuului vähän väliä puhetta, varmaan itä-eurooppalaista? en ole varma. Aaltopituus tässä on λ = 300 / f(MHz) = 300 / 14.208 ≈ 21,1 m. Kokeilin vaihtaa AM-napille: puhe vääristyi ja USB:llä taas selkeytyi.

<img width="1794" height="1338" alt="image" src="https://github.com/user-attachments/assets/0cd26004-8149-4cf4-af4c-695702ae796f" />

## c) ADS-B

Aloitin tehtävän menemällä ADS-B sivulle ja avasin siellä maailman kartan. Menin siellä Helsinki-Vantaan lentokentälle ja valitsin sieltä yhden koneen:

<img width="2590" height="1990" alt="image" src="https://github.com/user-attachments/assets/32ef2190-a52c-4fd5-84b3-92fcac2eea37" />

Kone on Air Chinan CCA912, joka on Airbus A350-900. Sen korkeus kuvanottohetkellä oli 37 000 ft ja sen nopeus 461 kt. Koneen suuntana on itä/kaakon suunta ja nykyinen sijainti oli Suomenlahdella eli on kai vain ohilennolla kohti itää.

Googlettaessa selvisi, että koneessa on ADS-B-transponderi, joka lähettää automaattisesti radiolla koneen tunnisteen, sijainnin, korkeuden ja nopeuden taajuudella 1090 MHz.

## d) Vapaaehtoinen Atalaya






 ## Lähteet

Karvinen, T. 2026. Verkkoon tunkeutuminen ja tiedustelu. Luettavissa: https://terokarvinen.com/verkkoon-tunkeutuminen-ja-tiedustelu/. Luettu: 22.10.2026.

Karvinen, T. 2026. Verkkoon tunkeutuminen ja tiedustelu. Luento. Haaga-Helia ammattikorkeakoulu. Luento 20.10.2026.

SKYbrary. Automatic Dependent Surveillance - Broadcast (ADS-B). Luettavissa: https://skybrary.aero/articles/automatic-dependent-surveillance-broadcast-ads-b. Luettu: 22.10.2026

University of Twente. WebSDR. Luettavissa: http://websdr.ewi.utwente.nl. Luettu: 22.10.2026.

https://excalidraw.com/
