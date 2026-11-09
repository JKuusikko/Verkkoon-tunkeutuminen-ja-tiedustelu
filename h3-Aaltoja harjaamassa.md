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

Analyysista sai seuraavia tuloksia:

- **House Code/ID**: 8785315
- **Unit**: 3
- **Channel**: 3
- **Command**: OFF
- **Group**: 0 ja **Group Call**: No

Eli rtl_433 tunnisti näytteestä kytkimen/pistorasian. Sama signaali tulkittiin kolmella eri protokollalla, joita on KlikAanKlikUit-Switch, Proove-Security ja Nexa-Security. Lähetettävä komento on OFF, joten tarkoituksena näyttäisi olevan laitteen sammuttaminen ja sama viesti esiintyy neljään kertaa tallenteessa.

## d) Too compex 16?

Tehtävässä on tarkoituksena ladata .complex16 file ja se muunnetaan rtl_433 yhteensopivaan muoton, jonka jälkeen se pitää vielä analysoida. Tehtävän aloitin lataamalla tiedoston komennolla `wget https://terokarvinen.com/verkkoon-tunkeutuminen-ja-tiedustelu/samples/Recorded-HackRF-20250411_183354-433_92MHz-2MSps-2MHz.complex16s`. Tämän jälkeen tiedosto pitää muuntaa rtl433 sovellukseen sopivaksi komennolla `cp Recorded-HackRF-20250411_183354-433_92MHz-2MSps-2MHz.complex16s tehtavaD_433.92M_2000k.cs8` ja sitten komennolla `rtl_433 -r tehtavaD_433.92M_2000k.cs8` tuli analyysi esiin:

<img width="1686" height="1422" alt="kuva" src="https://github.com/user-attachments/assets/7f915049-e377-4e6b-95c0-697f562ae05a" />

Analyysistä huomasin, että kyseessä on sama tiedosto kuin edelisessä kohdassa eli signaali tunnistui KlikAanKlikUit-Switch-, Proove-Security- ja Nexa-Security-yhteensopivaksi, House Code/ID on sama eli 8785315 ja komento OFF jne. Eli kyseessä tämän perusteella on sama kuin edellisessä kohdassa.

## d) Ultimate

<img width="1632" height="1210" alt="kuva" src="https://github.com/user-attachments/assets/f6dbd84e-047a-46d3-9855-cbd30605d6e2" />


<img width="2552" height="1654" alt="kuva" src="https://github.com/user-attachments/assets/1153fea6-dbf3-4a3a-8d10-a40f96439123" />









## Lähteet 

Hubacek. 2019. Universal Radio Hacker SDR Tutorial on 433 MHz radio plugs. YouTube. Katsottavissa: https://www.youtube.com/watch?v=sbqMqb6FVMY&t=199s. Katsottu: 6.11.2026.

Karvinen, T. 2026. Verkkoon tunkeutuminen ja tiedustelu (Converted_433.92M_2000k.cs8). Luettavissa: https://terokarvinen.com/verkkoon-tunkeutuminen-ja-tiedustelu/. Luettu: 9.11.2026.

Cornelius. 2022. Decode 433.92 MHz weather station data. Luettavissa: https://www.onetransistor.eu/2022/01/decode-433mhz-ask-signal.html. Luettu: 7.11.2026.

Lohner, K. 2019. Decoding ASK/OOK_PPM Signals with URH and rtl_433. Luettavissa: https://github.karllohner.com/SDR/Decoding/Example_2019-01-24/. Luettu: 7.11.2026.
