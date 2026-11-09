## Laitteisto

Tehtävät tein MacBook pro 2019, jossa macOS Tahoe 26.6. Virtual Machinena toimi VMware Fusion 26.0.0 ja distrona Kali Linux amd64. Tehtävät tein 6-8.11 välisenä aikana.


## x) Lue ja tiivistä

### Hubacek 2019: URH SDR Tutorial on 433 MHz radio plugs

- URH:lla voi nauhoittaa ja analysoida radiosignaalin.

- Signaalista demoduloidaan raakabitit, jotka näytetään esim. bitteinä ja heksana.

- Identtinen bittijono ja paketit ovat merkki yleensä siitä, että demodulaatio on tehty oikein ja ilman ongelmia.

- URH:n autodetect-ominaisuus on käyttäjälle apu analyysiä tehtäessä, mutta se ei kuitenkaan ole missään nimessä täydellinen ja siksi onkin erittäin kannattavaa toisinaan itse säädellä asetuksia.

### Cornelius 2022: Decode 433.92 MHz weather station data

- 433,92 MHz:n ISM-kaistalla lähettävät monet kuluttajalaitteet: sääasemat, lämpötila-anturit, ovikellot, rengaspaineanturit yms.

- Signaalin purkaminen etenee vaiheittain: nauhoitus -> modulaation tunnistus (ASK/OOK) -> lähetysformaatin purku.

- Käytetty modulaatio on ASK/OOK.

- Datassa on usein toistoa ja laitetunniste (device ID).

### Lohner 2019: Decoding ASK/OOK_PPM Signals with URH and rtl_433

- Artikkeli näyttää, miten valmiiksi nauhoitettuja I/Q-näytetiedostoja puretaan yhdistämällä URH ja rtl_433:n avulla.

- URH:ssa työvaiheet: lataa tiedosto -> Spectrogram-näkymä + kaistanpäästösuodatin poistamaan kohinaa -> säädä bit length ja error tolerance -> valitse muutama kokonainen symboli, josta lasketaan yhden symbolin pituus.

- Symbolin kesto ajassa: näytteiden määrä jaettuna näytteenottotaajuudella. Esimerkissä 252 näytettä 250k näytettä/s ≈ 1008 µs per symboli.

## b) rtl_433

Aloitin tehtävän päivittämällä paketit komennoilla `sudo apt update && sudo apt upgrade -y`. Sitten asensin ohjelman komennolla `sudo apt install rtl-433` ja komennolla `rtl_433` avasin ohjelman asennuksen jälkeen.

<img width="1172" height="184" alt="kuva" src="https://github.com/user-attachments/assets/7e65b0f6-0f02-4cea-a01d-332cf4260a5e" />

Ja avautuu, mutta kun ei ole mitään ylimääräisiä laitteita kiinni koneessa, niin tulee no supported devices found -ilmoitus.

## c) Automaattinen analyysi

Tehtävän tavoite on analysoida juuri ladatulla rtl_433 ohjelmalla **Converted_433.92M_2000k.cs8.** tiedostoa. Ensiksi latasin tiedoston virtuaalikoneelle komennolla `wget https://terokarvinen.com/verkkoon-tunkeutuminen-ja-tiedustelu/samples/Converted_433.92M_2000k.cs8`. Sitten komennolla `rtl_433 -r Converted_433.92M_2000k.cs8` tuli analyysi esille:

<img width="1672" height="1410" alt="kuva" src="https://github.com/user-attachments/assets/50f9b3d4-f3cc-4565-a489-32e0ef48c288" />

Analyysistä sai seuraavia tuloksia:

- **House Code/ID**: 8785315
- **Unit**: 3
- **Channel**: 3
- **Command**: OFF
- **Group**: 0 ja **Group Call**: No

Eli rtl_433 tunnisti näytteestä kytkimen/pistorasian. Sama signaali tulkittiin kolmella eri protokollalla, joita on KlikAanKlikUit-Switch, Proove-Security ja Nexa-Security. Lähetettävä komento on OFF, joten tarkoituksena näyttäisi olevan laitteen sammuttaminen ja sama viesti esiintyy neljään kertaa tallenteessa.

## d) Too complex 16?

Tehtävässä on tarkoituksena ladata .complex16 file ja se muunnetaan rtl_433 yhteensopivaan muotoon, jonka jälkeen se pitää vielä analysoida. Tehtävän aloitin lataamalla tiedoston komennolla `wget https://terokarvinen.com/verkkoon-tunkeutuminen-ja-tiedustelu/samples/Recorded-HackRF-20250411_183354-433_92MHz-2MSps-2MHz.complex16s`. Tämän jälkeen tiedosto pitää muuntaa rtl433 sovellukseen sopivaksi komennolla `cp Recorded-HackRF-20250411_183354-433_92MHz-2MSps-2MHz.complex16s tehtavaD_433.92M_2000k.cs8` ja sitten komennolla `rtl_433 -r tehtavaD_433.92M_2000k.cs8` tuli analyysi esiin:

<img width="1686" height="1422" alt="kuva" src="https://github.com/user-attachments/assets/7f915049-e377-4e6b-95c0-697f562ae05a" />

Analyysistä huomasin, että kyseessä on sama tiedosto kuin edellisessä kohdassa eli signaali tunnistui KlikAanKlikUit-Switch-, Proove-Security- ja Nexa-Security-yhteensopivaksi, House Code/ID on sama eli 8785315 ja komento OFF jne. Eli kyseessä tämän perusteella on sama kuin edellisessä kohdassa.

## e) Ultimate

Tässä kohdassa asennetaan virtuaalikoneelle **URH** joka on lyhenne **Universal Radio Hacker**. Aloitin sen asentamalla ensin pipx komennolla `sudo apt-get -y install pipx` ja sitten `pipx install urh`, joka asensi itse urh:n.

<img width="1632" height="1210" alt="kuva" src="https://github.com/user-attachments/assets/f6dbd84e-047a-46d3-9855-cbd30605d6e2" />

Sitten komennolla `urh` niin aukesi:

<img width="2552" height="1654" alt="kuva" src="https://github.com/user-attachments/assets/1153fea6-dbf3-4a3a-8d10-a40f96439123" />

Näyttää oikealta ja tehtävä siis tehty.

## f) yleiskuva

Tämä kohta jaettu f ja g kohtaan ja niiden tavoitteena on tarkastella näytettä **1-on-on-on-HackRF-20250412_113805-433_912MHz-2MSps-2MHz.complex16s** . f kohdassa katsotaan yleiskuvaa ja g kohdassa demoduloidaan signaali raakabiteiksi.

Aloitin f kohdan lataamalla näytteen komennolla `wget https://terokarvinen.com/verkkoon-tunkeutuminen-ja-tiedustelu/samples/1-on-on-on-HackRF-20250412_113805-433_912MHz-2MSps-2MHz.complex16s`. Avasin näytteen urh:ssa:

<img width="2550" height="1520" alt="kuva" src="https://github.com/user-attachments/assets/c6265dc2-18d7-4813-9730-090ecbef3945" />

Tehtävänannossa mainitaan, että näytteessä Nexan pistorasian kaukosäätimen valon 1 ON -nappia on painettu kolmesti, joka näkyy kuvassa. Tiedoston perusteella voi päätellä, että tallennus on tehty 12.4.2025 klo 11:38:05 taajuudella 433,912 MHz ja näytteenottotaajuudella 2 MS/s. Kuten mainitsin niin näytteestä näkee kolme lähetysaaltoa, jotka näyttää samanlaisilta. Tämä vastaa tehtävänantoa. Koko näytteen pituus on noin 5,37 sekuntia, joka on siis viimeisen bitin aikaleima.

## g) Bittistä

Tässä kohdassa demoduloidaan signaali raakabiteiksi ja sen jälkeen pohtia mikä on oikea modulaatio ja löytää bitin pituus ajassa. Aluksi modulaatioksi ASK (Amplitude Shift Keying / OOK), jossa tieto koodataan sillä, onko kantoaalto päällä vai pois ja Bits/Symbol-arvoksi 1. Painoin “Autodetect parameters”, joka piti Samples/Symbol-arvona 500.

<img width="2542" height="1412" alt="kuva" src="https://github.com/user-attachments/assets/7961a079-6055-4a1d-9c35-d27d28eb7388" />

Yhden raakabitin kesto ajassa lasketaan jakamalla symbolin pituus näytteenottotaajuudella:

Samples/Symbol = 500 näytettä -> Näytteenottotaajuus = 2 MS/s = 2 000 000 näytettä/s -> Bitin kesto = 500 ÷ 2 000 000 = 0,00025 s = 250 µs. Tästä en ole satavarma onko oikein, mutta tämän vastauksen minä sain. 250 µs on neljäsosa millisekunnista ja esimerkiksi silmänräpäykseen (~100 ms) mahtuisi satoja tällaisia bittejä.

## h) SDR++

Asensin SDR++:n komennolla `sudo apt install sdrpp` ja käynnistin sen. Lisäsin signaalilähteeksi File Source -moduulin ja valitsin siihen aiemman Nexa-näytteeni ja asetin formaatin.

<img width="2544" height="1660" alt="kuva" src="https://github.com/user-attachments/assets/0b57add4-c555-470e-9d47-615aedb3844b" />

Signaalin saaminen toistumaan ei kuitenkaan onnistunut ja lokiin tuli virhe “Error: Sample rate may not be zero”. Googlettamalla selisi, että SDR++:n File Source ei lukenut näytteenottotaajuutta tiedostosta, joten toistonopeus jäi nollaksi. Kokeilin korjata asiaa nimeämällä tiedoston uudelleen, mutta tämä versio ei tukenut sitä. Ohjelma itsessään kuitenkin toimii ja sen käyttöliittymä on valmis.

## i) GNU radio

Asensin GNU Radion komennolla `sudo apt install gnuradio` ja käynnistin sen komennolla `gnuradio-companion`:

<img width="1846" height="1264" alt="kuva" src="https://github.com/user-attachments/assets/0f38fea7-ca22-458c-96bf-1a414a0e4d8d" />

Sen jälkeen tein yksinkertaisen flowgraphin, tallensin sen ja ajoin:

<img width="1842" height="1080" alt="kuva" src="https://github.com/user-attachments/assets/0b126168-ff0b-4aec-bc20-4cc1875070f0" />

Flowgraph tuottaa siniaallon (Signal Source) ja piirtää sen aikatasossa (QT GUI Time Sink). Kuvan sininen (Signal 1) ja punainen (Signal 2) käyrä ovat kompleksisignaalin I- ja Q-komponentit.

<img width="1822" height="1270" alt="kuva" src="https://github.com/user-attachments/assets/0bc2a034-5155-464d-95a6-fe0b553ea846" />

## Lähteet

AlexandreRouma. SDR++. GitHub. Luettavissa: https://github.com/AlexandreRouma/SDRPlusPlus. Luettu: 9.11.2026.

Anthropic. 2026. Claude. Käytetty apuna rtl_433- ja URH-analyysin tulkinnassa sekä käsitteiden avaamisessa. Keskustelut käyty: 7-9.11.2026.

Cornelius. 2022. Decode 433.92 MHz weather station data. Luettavissa: https://www.onetransistor.eu/2022/01/decode-433mhz-ask-signal.html. Luettu: 6.11.2026.

GNU Radio Project. 2026. GNU Radio. Luettavissa: https://www.gnuradio.org/. Luettu: 9.11.2026.

Hubacek. 2019. Universal Radio Hacker SDR Tutorial on 433 MHz radio plugs. YouTube. Katsottavissa: https://www.youtube.com/watch?v=sbqMqb6FVMY&t=199s. Katsottu: 9.11.2026.

Karvinen, T. 2026. Verkkoon tunkeutuminen ja tiedustelu (tehtävänanto). Luettavissa: https://terokarvinen.com/verkkoon-tunkeutuminen-ja-tiedustelu/. Luettu: 6.11.2026.

Karvinen, T. 2026. Converted_433.92M_2000k.cs8 (näytetiedosto). Luettavissa: https://terokarvinen.com/verkkoon-tunkeutuminen-ja-tiedustelu/samples/Converted_433.92M_2000k.cs8. Luettu: 8.11.2026.

Karvinen, T. 2026. Recorded-HackRF-20250411_183354-433_92MHz-2MSps-2MHz.complex16s (näytetiedosto). Luettavissa: https://terokarvinen.com/verkkoon-tunkeutuminen-ja-tiedustelu/samples/Recorded-HackRF-20250411_183354-433_92MHz-2MSps-2MHz.complex16s. Luettu: 9.11.2026.

Karvinen, T. 2026. 1-on-on-on-HackRF-20250412_113805-433_912MHz-2MSps-2MHz.complex16s (näytetiedosto). Luettavissa: https://terokarvinen.com/verkkoon-tunkeutuminen-ja-tiedustelu/samples/1-on-on-on-HackRF-20250412_113805-433_912MHz-2MSps-2MHz.complex16s. Luettu: 9.11.2026.

Lohner, K. 2019. Decoding ASK/OOK_PPM Signals with URH and rtl_433. Luettavissa: https://github.karllohner.com/SDR/Decoding/Example_2019-01-24/. Luettu: 6.11.2026.

merbanan. rtl_433. GitHub. Luettavissa: https://github.com/merbanan/rtl_433. Luettu: 7.11.2026.

Pohl, J. Universal Radio Hacker (URH). GitHub. Luettavissa: https://github.com/jopohl/urh. Luettu: 9.11.2026.
