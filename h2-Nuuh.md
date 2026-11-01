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

<img width="1680" height="1170" alt="kuva" src="https://github.com/user-attachments/assets/a5f3e4f8-6eb4-4f9b-9ed1-07029a62ee3f" />

- Ethernet -> linkkikerros: siirtää kehyksen lähiverkossa MAC-osoitteiden avulla (Src VMware_e4:26:ee -> Dst VMware_fd:db:7b)

<img width="1598" height="174" alt="kuva" src="https://github.com/user-attachments/assets/11a03635-ea49-409e-8482-2ce79e354ee3" />

- Internet Protocol Version 4 -> internet kerros: reitittää paketin IP-osoitteiden avulla (108.177.14.113 -> 192.168.2.128).

<img width="1606" height="514" alt="kuva" src="https://github.com/user-attachments/assets/96ee9ef4-970e-498c-93f1-96881e752086" />

- Transmission Control Protocol -> kuljetuskerros: huolehtii yhteydestä päästä päähän ja porteista, tässä tapauksessa 443 = HTTPS.

<img width="1630" height="784" alt="kuva" src="https://github.com/user-attachments/assets/39dbd374-8711-4ba6-9d2c-fb66fdbc1d1d" />

- Transport Layer Security -> sovelluskerros: varsinainen sovellusprotokolla, tässä salattu TLS-liikenne.

<img width="1602" height="264" alt="kuva" src="https://github.com/user-attachments/assets/8bac8a21-e553-4870-83e3-f88885de2a94" />

Ylin Frame-rivi ei ole varsinainen kerros vaan Wiresharkin metatieto kaapatusta kehyksestä. Koska liikenne oli HTTPS:ää, sovelluskerros on salattua, joten itse sisältöä ei näe.
















## Lähteet

Karvinen, T. 2025. Wireshark - Getting Started. Luettavissa: https://terokarvinen.com/wireshark-getting-started/. Luettu: 1.11.2026.

Karvinen, T. 2025. Network Interface Names on Linux. Luettavissa: https://terokarvinen.com/network-interface-linux/. Luettu: 1.11.2026.
