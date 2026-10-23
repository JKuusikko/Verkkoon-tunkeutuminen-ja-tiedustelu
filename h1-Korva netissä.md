## Ympäristö

Päälaitteenani oli MacBook Pro (M2), jossa macOS Tahoe 26.7.1. Kakkoskoneenani on MacBook Pro intel versio, jossa sama MacOS Tahoe 26.7.1. Normaalisti minulla on iPhone, mutta löysin kaapinpohjalta vanhan Android-puhelimen - Honor 10, jossa Android 10. Honorini oli aika vanha, mutta jollain ihmeellä kaikki toimi niin kun piti :).

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

Asensin F-Droidin ja sen kautta Atalaya-sovelluksen. Annoin sijaintiluvan ja laitoin SIM-kortin paikalleen, jotta tukiasematiedot näkyvät.

<img width="743" height="1568" alt="image" src="https://github.com/user-attachments/assets/2f6b1502-510e-45f1-bb61-a1264f1e734d" />

<img width="743" height="1568" alt="image" src="https://github.com/user-attachments/assets/fa39290c-faf7-4deb-a5d0-4f69b6314d79" />

<img width="743" height="1568" alt="image" src="https://github.com/user-attachments/assets/ad4ad2ab-be0c-4836-8c36-41d144cefe57" />

Huomasin, että olin kiinni Telian 4G LTE -verkossa, kaistalla B3 (1800 MHz). Tukiaseman/solun tunnisteet: CI 37652757, eNB 147081, PCI 355, TAC 4220, CID 21. Signaaliarvot: RSSI −103 dBm, RSRP −103 dBm, RSRQ −10 dB, SNR 8 dB. Cells-näkymässä näkyi useita naapurisoluja (PCI 355, 374, 366, 385), kaikki B3/1800 MHz, eri signaalitasoilla.

Mielenkiintoista miten pelkällä ilmaisella sovelluksella ja SIM-kortilla näkee tarkasti mihin mastoon on kiinni, millä taajuudella ja montako sektoria ympärillä on.

## e) Vapaaehtoinen WifiAnalyzer

Avasin WifiAnalyzerin ja tarkastelin ympäröiviä WiFi-verkkoja kolmella taajuusalueella (2,4 GHz, 5 GHz ja 6 GHz) sekä eri näkymissä (Access Points, Channel Graph, Channel Rating).

<img width="743" height="1568" alt="image" src="https://github.com/user-attachments/assets/b86b9f57-422c-414f-9cb8-5994414df6ed" />

Oma verkkoni oli kiinni 5 GHz:llä kanavalla 100 (5500 MHz), vahvalla signaalilla ja suojauksena WPA2/WPA3. Sama verkko näkyi myös 2,4 GHz:llä kanavalla 6.

<img width="743" height="1568" alt="image" src="https://github.com/user-attachments/assets/06f61ad0-08e6-4cc9-8643-8a650dadd848" />

2,4 GHz -alueella verkot kasautuivat kanaville 6, 10 ja 13 ja menivät osittain päällekkäin.

<img width="743" height="1568" alt="image" src="https://github.com/user-attachments/assets/d7931d7d-dfbc-480e-a834-de171c860d02" />

5 GHz -alueella oli selvästi enemmän tilaa ja lähes kaikki kanavat olivat vapaita. 6 GHz -alueella näkyi rasti, eli todennäköisesti vanha Honor 10 -puhelimeni ei tue sitä.

Oli tosi jännä, että sovelluksella näkee kaikki ympäröivät verkot, niiden kanavat, signaalivahvuudet, suojaustyypit ja jopa piilotetut verkot.

## f) Vapaaehtoinen GPSTest

Asensin F-Droidin kautta GPSTest-sovelluksen, annoin sijaintiluvan ja menin pihalle, jotta satelliittisignaali kuuluu paremmin. Sain paikannuksen (fix) noin 2 sekunnissa, ja vaakatarkkuudeksi tuli noin 14 metriä.

<img width="1080" height="1003" alt="image" src="https://github.com/user-attachments/assets/d92b2bf1-bec2-480b-a6e5-a9a2910f1e21" />

<img width="743" height="1568" alt="image" src="https://github.com/user-attachments/assets/001c7574-5d1f-4566-9a76-3aa19e61ad9d" />

Näkyvissä oli 15 satelliittia kahdesta eri GNSS-järjestelmästä: USA:n ja Venäjän. Taulukossa näkyi jokaiselle satelliitille korkeuskulma (Kork.), suunta (Atsim) ja ratatietoliput (A = almanakka, E = efemeridi). SBAS-tukisatelliitteja ei ollut saatavilla sijainnissani.

 ## Lähteet

ADS-B Exchange. Luettavissa: https://globe.adsbexchange.com/. Luettu: 22.10.2026.

Excalidraw. Luettavissa: https://excalidraw.com/. Luettu: 23.10.2026.

F-Droid. Atalaya. Luettavissa: https://f-droid.org/packages/cat.naval.atalaya/. Luettu: 23.10.2026.

F-Droid. GPSTest. Luettavissa: https://f-droid.org/packages/com.android.gpstest/. Luettu: 23.10.2026.

F-Droid. WiFiAnalyzer. Luettavissa: https://f-droid.org/packages/com.vrem.wifianalyzer/. Luettu: 23.10.2026.

Karvinen, T. 2026. Verkkoon tunkeutuminen ja tiedustelu. Luettavissa: https://terokarvinen.com/verkkoon-tunkeutuminen-ja-tiedustelu/. Luettu: 22.10.2026.

Karvinen, T. 2026. Verkkoon tunkeutuminen ja tiedustelu. Luento. Haaga-Helia ammattikorkeakoulu. Luento 20.10.2026.

SKYbrary. Automatic Dependent Surveillance - Broadcast (ADS-B). Luettavissa: https://skybrary.aero/articles/automatic-dependent-surveillance-broadcast-ads-b. Luettu: 22.10.2026

University of Twente. WebSDR. Luettavissa: http://websdr.ewi.utwente.nl:8901 Luettu: 22.10.2026.

