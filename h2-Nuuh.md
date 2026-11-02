## Laitteisto

Tehtävät tein MacBook pro 2019, jossa macOS Tahoe 26.6. Virtual Machinena toimi VMware Fusion 26.0.0 ja distrona Kali Linux amd64.

## X) Lue ja tiivistä

### Wireshark - Getting started

- Wireshark on johtava verkkoliikenteen `nuuskija` ja `analysaattori`, asennus: `sudo apt-get install wireshark`.

- Oma käyttäjä lisätään komennolla `sudo adduser "käyttäjän nimi" wireshark`, jotta kaappaus onnistuu ilman root-oikeuksia.

- Valitse kaapattava verkkoliitäntänä sellainen jossa on liikennettä, any-filtteri kaappaa liikennettä kaikista verkoista.

- Kaappauksen jälkeen Statistics-valikosta näkee yleiskuvan ja display-suodattimilla (dns, tls, http, tcp.port==443, ip.addr==…, frame contains "…") rajataan näkyviin vain haluttu liikenne.

### Network Interface Names on Linux

- Verkkoliitäntä on kuin “verkkokortti”, mutta ei välttämättä fyysinen, esim. loopback (lo) on virtuaalinen, localhostia (127.0.0.1) varten.

- Nykyisten (systemd-pohjaisten) jakeluiden nimeäminen etuliitteen mukaan:
  - en = langallinen Ethernet (esim. enp1s0)
  - wl = WiFi (esim. wlp4s0)
  - lo = loopback

- Vanhat nimet, esim. eth0 tai wlan ovat enimmäkseen poistuneet, koska rinnakkaisessa boottauksessa ne sekoittuisivat; uudet nimet pysyvät samoina laitteelle myös uudelleenkäynnistyksissä.

- Omat liitännät näkee komennoilla `ip a` ja `ip route`.

## a) Linux

Minulla on jo Kali Linux asennettu aikaisemmilta Teron kursseilta. Laitoin virtuaalikoneelle 8GB RAMia, 40GB muistia ja 2 processoria. Asennuksesta on jo aikaa, mutta muistaakseni asennuksessa ei ollut mitään ongelmia.

## b) Ei voi kalastaa

Aloitin tehtävän komennolla `ip a`, jolla näin kaikki verkkoyhteydet ja ainut mikä oli päällä oli eth0.

<img width="1658" height="736" alt="kuva" src="https://github.com/user-attachments/assets/ee91666b-75ea-49a6-a9d6-00e10e986959" />

Sen jälkeen ajoin komennon `sudo ip link set eth0 down` ja sitten `ping 8.8.8.8` ja sieltä tuli viesti "ping: connect: Network is unreachable". Sitten laitoin verkon taas päälle komennolla `sudo ip link set eth0 up`ja sitten kun tein saman pingin niin tuli liikennettä: PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data. 64 bytes from 8.8.8.8: icmp_seq=1 ttl=128 time=4.27 ms.

<img width="1664" height="1180" alt="kuva" src="https://github.com/user-attachments/assets/bd918ee6-fbf2-4385-bbab-1ba99245c205" />

## c) Wireshark

Wireshark oli jo valmiiksi ladattu minulla Kali Linuxissa. Varmistin groups-komennolla, että käyttäjäni kuuluu wireshark-ryhmään. Sitten avasin wiresharkin terminaalista komennolla `wireshark` ja se avautui. Sitten valitsin oman verkkoliitännän eth0 ja rupesin kaappaamaan omaa liikennettä. Menin sitte firefoxissa terokarvinen.com sivulle.

<img width="1518" height="1182" alt="kuva" src="https://github.com/user-attachments/assets/4f22db60-dfd8-4ad4-a7a7-44edc305b40c" />

ja siellähän tuli liikennettä paljon ja kuvan alhaalla näkyy, että on 3797 pakettia napattu.

## d) Oikeesti TCP/IP

Valitsin yhden TLS-paketin edellisestä kohdasta (3505), josta näkyvät kaikki TCP/IP-mallin neljä kerrosta.

<img width="1796" height="842" alt="kuva" src="https://github.com/user-attachments/assets/8e00139d-d39f-4ce7-8a64-616f23457b35" />


- Ethernet -> linkkikerros: siirtää kehyksen lähiverkossa MAC-osoitteiden avulla (Src VMware_e4:26:ee -> Dst VMware_fd:db:7b)

<img width="1598" height="174" alt="kuva" src="https://github.com/user-attachments/assets/11a03635-ea49-409e-8482-2ce79e354ee3" />

- Internet Protocol Version 4 -> internet kerros: reitittää paketin IP-osoitteiden avulla (108.177.14.113 -> 192.168.2.128).

<img width="1606" height="514" alt="kuva" src="https://github.com/user-attachments/assets/96ee9ef4-970e-498c-93f1-96881e752086" />

- Transmission Control Protocol -> kuljetuskerros: huolehtii yhteydestä päästä päähän ja porteista, tässä tapauksessa 443 = HTTPS.

<img width="1630" height="784" alt="kuva" src="https://github.com/user-attachments/assets/39dbd374-8711-4ba6-9d2c-fb66fdbc1d1d" />

- Transport Layer Security -> sovelluskerros: varsinainen sovellusprotokolla, tässä salattu TLS-liikenne.

<img width="1602" height="264" alt="kuva" src="https://github.com/user-attachments/assets/8bac8a21-e553-4870-83e3-f88885de2a94" />

Ylin Frame-rivi ei ole varsinainen kerros vaan Wiresharkin metatieto kaapatusta kehyksestä. Koska liikenne oli HTTPS:ää, sovelluskerros on salattua, joten itse sisältöä ei näe.

## e) Mitäs tuli surffattua?

Aloitin tehtävän lataamalla Teron sivulta tiedoston ja avasin sen Wiresharkissa.

<img width="1520" height="972" alt="kuva" src="https://github.com/user-attachments/assets/d1a4a743-1cbb-4291-92b1-e6f3bbbc74c3" />

Kuvasta näkyy, että on kaapattu liikennettä, jossa on 283 pakettia eli mielestäni aika vähän liikennettä. Kun katsoin statistics välilehdeltä tietoja niin sieltä löytyi muun muassa 7 eri ip-osoitetta ja 2 MAC-osoitteita eli todennäköisesti kaksi laitetta. Näistä aktiivisin kone oli 192.168.122.7, jossa 281 pakettia.

<img width="1506" height="918" alt="kuva" src="https://github.com/user-attachments/assets/cdb46276-83ec-46ab-acea-3ffe058787b9" />

Protokollista silmään pisti DNS, QUIC, TCP, UDP, TLSv1.3, ARP. Koko ja kesto oli 126 kB ja 7,5 sekuntia. Ajankohta 28.3.2025 klo 17:28.

## f) Vapaaehtoinen, vaikea: Mitä selainta käyttäjä käyttää?

Koska liikenne oli pääosin salattua, selainta ei näe suoraan. Etsin TLS paketista JA3-sormenjäljen, joka tiivistää TLS-kättelyn ominaispiirteet ja toimii ohjelman “sormenjälkenä”.

<img width="1450" height="428" alt="kuva" src="https://github.com/user-attachments/assets/8e77ecba-ef3b-4c8a-8a1c-c102a1ce264e" />

JA3-arvo: `b5001237acdf006056b409cc433726b0`, tässä vaiheessa osaamiseni loppu, joten katsoi lopun tehtävästä Teron sivulta olevasta linkistä ja siellä kerrottiin sivu, jossa voi verrata JA3 arvoja. 

<img width="2096" height="1084" alt="kuva" src="https://github.com/user-attachments/assets/677c8dea-6452-4b58-a36c-6ce83ef7b075" />

Tästä johtopäätöksenä käyttäjä käyttää todennäköisesti Firefoxia. Kuitenkin ei ole 100% varmuudella, koska siellä näkyi myös muitakin selaimia, mutta pääasiassa 95-98% käytti Firefoxia.

## g) Minkä merkkinen verkkokortti käyttäjällä on?

Katsoin käyttäjän koneen (192.168.122.7) lähettämän paketin Ethernet II -kerroksesta Source-MAC-osoitteen: 52:54:00:2f:e1:e5.

 <img width="1496" height="366" alt="kuva" src="https://github.com/user-attachments/assets/d9265406-78fb-4185-a2c2-64fd97ab87f3" />

MAC-osoitteen kolme ensimmäistä tavua kertovat yleensä verkkokortin valmistajan. Tässä kuitenkin LG-bitti on 1 (“Locally administered address”), eli osoite ei ole valmistajan antama vaan ohjelmallisesti asetettu. Tämä tarkoittaa, että käyttäjällä ei ole tunnistettavaa fyysistä verkkokorttimerkkiä vaan kyseessä on todennäköisesti virtuaalikoneen emuloima verkkokortti. Käyttäjä on siis surffannut virtuaalikoneesta käsin. Tästä en ole kuitenkaan 100% varma vaan tiedon tästä etsin netistä.

## h) Millä weppipalvelimella käyttäjä on surffaillut?

Asetin filtteriin `dns.qry.name` ja sain suodateetua DNS-kyselyt, joista selviää että käyttäjä käynyt google.com:ssa sekä terokarvinen.com:ssa.

<img width="1914" height="970" alt="kuva" src="https://github.com/user-attachments/assets/0f015eae-c377-41c7-bb0b-e092e2228364" />

## i) Analyysi omasta liikenteestä

Tehtävässä käytin aikaisemmissa tehtävissä käytettyä kaappausta. Otin tarkasteluun kaksi pakettia: DNS-kyselyn ja siihen tulleen vastauksen (Transaction ID 0x99e9).

<img width="1972" height="640" alt="kuva" src="https://github.com/user-attachments/assets/4d650487-2a42-4cf0-913d-332bde34a655" />

- Frame: paketin koko oli 76 tavua. Tämä on Wiresharkin metatieto, ei varsinainen protokollakerros.

- Ethernet II (linkkikerros): lähde-MAC 00:0c:29:fd:db:7b = oma koneeni, kohde-MAC 00:50:56:e4:26:ee (VMware_e4:26:ee) = gateway/NAT. Type-kenttä 0x0800 kertoo, että sisällä on IPv4-paketti. Tämä kerros siirtää kehyksen lähiverkossa fyysisten osoitteiden avulla.

- IP (internet-kerros): lähde [täytä, oma IP] → kohde [täytä, nimipalvelimen IP]. TTL [täytä]. Protocol-kenttä 17 = UDP. Tämä kerros reitittää paketin IP-osoitteiden avulla.







## Lähteet

Karvinen, T. 2025. Wireshark - Getting Started. Luettavissa: https://terokarvinen.com/wireshark-getting-started/. Luettu: 1.11.2026.

Karvinen, T. 2025. Network Interface Names on Linux. Luettavissa: https://terokarvinen.com/network-interface-linux/. Luettu: 1.11.2026.

Karvinen, T. 2026. Verkkoon tunkeutuminen ja tiedustelu (surfing-secure.pcap). Luettavissa: https://terokarvinen.com/verkkoon-tunkeutuminen-ja-tiedustelu/. Luettu: 1.11.2026.

ja3.zone

 Läpikävely selaimen selvittämiseen kohdassa f. Giang 2025: H1 Sniff: f) Mitä selainta käyttäjä käyttää? (spoiler)
