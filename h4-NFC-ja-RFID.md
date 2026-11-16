## Laitteisto

Tehtävät tein MacBook pro 2019, jossa macOS Tahoe 26.6.

## a) Tarkastele käytössäsi olevia RFID tuotteita, mieti miten hyvin olet suojautunut RFID urkinnalta?

Kun mietin ja tarkastelin niin huomasin, että minulla on käytössä seuraavat RFID/NFC-tuotteet/laitteet:

- **Apple Pay**: Maksaessani eri ostoksiani niin käytän pääasiassa puhelimen Apple Payta. Se on turvallisin vaihtoehto skimmingiä vastaan, koska se ei lähetä korttinumeroa lainkaan. Maksu perustuu tokenisointiin eli puhelimeen tallennetaan erillinen “device account number” ja jokainen maksu vahvistetaan kertakäyttöisellä kryptogrammilla ja biometrialla (Face ID). Vaikka liikennettä katseltaisiin, korttitietoja ei saa ja  maksua on mahdotonta toistaa.

- **Pankkikortit**: Minulla on myös käytössä muutamia muita pankkikortteja. Näissä on käytössä NFC-lähimaksu, joka voi olla perinteinen skimming-kohde. Suojana on kuitenkin se, että lähimaksu ilman PINiä on rajattu pieniin summiin ja muutaman oston jälkeen pankki vaatii PINin. Skimmerin pitää myös olla hyvin lähellä korttia.

- **Passi**: Sirulla on henkilötietoni, mutta sitä ei voi lukea ilman passin koneluettavan rivin tietoja (BAC/PACE-suojaus) eli passi pitää käytännössä avata lukemista varten. Pelkkä ohimennen skannaus ei toimisi.

- **Auton avaimet**: Tässä on mielestäni suurin riski. Keyless-avaimet ovat alttiita relay-hyökkäykselle, jossa hyökkääjät pidentävät avaimen signaalin ovelta autolle ja voivat avata tai käynnistää auton, vaikka avain on sisällä.

- **Vanhat kulkulätkät**: Minulla on vanhoja kulkukortteja, jotka eivät ole enää aktiivisia. Tällaiset (usein MIFARE Classic) ovat helposti kloonattavissa halvalla laitteella, ja vanha MIFARE Classic on jo murrettu. Koska ne eivät ole käytössä, riski on pieni, mutta hyvä käytäntö olisi hävittää vanhat tunnisteet.

Kun näitä tuli mietittyä niin pääasiassa suojaus on hyvällä tasolla, toki voisi parantaa esim avainten suojausta eli pitää kauempana ulko-ovea tai hävittää vanhat kulkulätkät. Maksamisen osalta olen mielestäni hyvin suojautunut, koska käytän eniten Apple Payta, joka ei paljasta korttitietoja ja vaatii biometrisen vahvistuksen.

## b) Tutustu APDU komentojen rakenteeseen

APDU eli Application Protocol Data Unit on sirukortin (smart card / NFC) ja lukijan välisen viestinnän perusyksikkö. Kommunikointi on aina request-asnwer pari eli lukija lähettää Command APDU:n ja kortti vastaa Response APDU:lla.

### Command APDU

Lukijalta -> kortille. Rakenne jakautuu otsikkoon (header), joka on pakollinen ja runkoon (body), joka on valinnainen:

| Kenttä | Koko | Merkitys |
|---|---|---|
| CLA | 1 tavu | Komentoluokka eli onko standardi vai korttikohtainen komento |
| INS | 1 tavu | Instruction eli itse komento (esim. SELECT, READ, VERIFY) |
| P1 | 1 tavu | Parametri 1 –> komennon tarkennus |
| P2 | 1 tavu | Parametri 2 –> komennon tarkennus |
| Lc | 0–3 tavua | Lähetettävän datan pituus |
| Data | Lc tavua | Varsinainen lähetettävä data |
| Le | 0–3 tavua | Odotetun vastausdatan pituus |

### Response APDU

Kortilta -> lukijalle:

| Kenttä | Koko | Merkitys |
|---|---|---|
| Data | vaihtelee | Vastausdata (valinnainen) |
| SW1 | 1 tavu | Status word 1 -> tilakoodin ensimmäinen tavu |
| SW2 | 1 tavu | Status word 2 –> tilakoodin toinen tavu |

SW1–SW2 on aina mukana ja kertoo miten komento meni. Esimerkkejä:

- 90 00 = onnistui (OK)
- 6A 82 = tiedostoa ei löydy
- 63 Cx = väärä PIN, x yritystä jäljellä
- 67 00 = väärä pituus

## c) Tutki ja kerro minkä mielenkiintoisen RFID hakkerointi uutiset löysit

Löysin kiinnostavan uutisen Shanghai Fudan Microelectronicsin RFID-korttien takaovesta (Security Affairs, 2024).

Tietoturvayhtiö Quarkslabin tutkijat löysivät laitteistotason takaoven miljoonista kiinalaisvalmisteisista RFID-älykorteista **(mallit FM11RF08S ja FM11RF08)**. Mallit ovat MIFARE Classic -yhteensopivia. Takaovi on salainen todennuskomento, jonka avain on sama kaikissa korteissa. Tutkijat onnistuivat murtamaan tämän avaimen.

Hyökkäys on vakava, koska takaoven tunteva pystyy kloonaamaan kortin muutamassa minuutissa pelkällä hetkellisellä pääsyllä korttiin ilman ennakkotietoa kortin omista salausavaimista. Erityisen ironista tässä on se, että FM11RF08S-kortti suunniteltiin alun perin suojaamaan MIFARE Classicin tunnettuja heikkouksia vastaan, mutta samaan siruun lisättiin takaovi, joka romuttaa suojauksen.

Kyseessä on toimitusketjuhaavoittuvuus: vika on itse sirussa, ei käyttäjän toiminnassa. Ongelma on laaja, koska monet eivät tiedä korttiensa olevan juuri näitä Fudanin siruja. Niitä löytyi muun muassa hotelleista Yhdysvalloissa, Euroopassa ja Intiassa.

## Lähteet

Ambimat Electronics. Smart Card Selected APDU Commands. Luettavissa: https://ambimat.com/smart-card-selected-apdu-commands/. Luettu: 15.11.2026.

Anthropic. 2026. Claude. Käytetty APDU-komentojen rakenteen selittämisessä. Keskustelut käyty: 16.11.2026.

CardLogix. APDU – Application Protocol Data Unit (Smart Card). Luettavissa: https://www.cardlogix.com/glossary/apdu-application-protocol-data-unit-smart-card/. Luettu: 16.11.2026.

Karvinen, T. 2026. Verkkoon tunkeutuminen ja tiedustelu (tehtävänanto). Luettavissa: https://terokarvinen.com/verkkoon-tunkeutuminen-ja-tiedustelu/. Luettu: 15.11.2026.

Paganini, P. 2024. A backdoor in millions of Shanghai Fudan Microelectronics RFID cards allows cloning. Security Affairs. Luettavissa: https://securityaffairs.com/167321/hacking/shanghai-fudan-microelectronics-rfid-cards-backdoor.html. Luettu: 16.11.2026.

