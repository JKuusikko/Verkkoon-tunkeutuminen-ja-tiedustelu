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

- Vanhat nimet (eth0, wlan0) ovat enimmäkseen poistuneet, koska rinnakkaisessa boottauksessa ne sekoittuisivat; uudet nimet pysyvät samoina laitteelle myös uudelleenkäynnistyksissä.

- Omat liitännät näkee komennoilla `ip a` ja `ip route`.


## a) Linux

Minulla on jo Kali Linux asennettu aikaisemmilta Teron kursseilta. Laitoin virtuaalikoneelle 8GB RAMia, 40GB muistia ja 2 processoria. Asennuksesta on jo aikaa, mutta muistaakseni asennuksessa ei ollut mitään ongelmia.

## b) Ei voi kalastaa

Aloitin tehtävän komennolla `ip a`, jolla näin kaikki verkkoyhteydet ja ainut mikä oli päällä oli eth0.

<img width="1658" height="736" alt="kuva" src="https://github.com/user-attachments/assets/ee91666b-75ea-49a6-a9d6-00e10e986959" />

Sen jälkeen ajoin komennon `sudo ip link set eth0 down` ja sitten `ping 8.8.8.8` ja sieltä tuli viesti "ping: connect: Network is unreachable". Sitten laitoin verkon taas päälle komennolla `sudo ip link set eth0 up`ja sitten kun tein saman pingin niin tuli liikennettä: PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data. 64 bytes from 8.8.8.8: icmp_seq=1 ttl=128 time=4.27 ms.

<img width="1664" height="1180" alt="kuva" src="https://github.com/user-attachments/assets/bd918ee6-fbf2-4385-bbab-1ba99245c205" />








## Lähteet

Karvinen, T. 2025. Wireshark - Getting Started. Luettavissa: https://terokarvinen.com/wireshark-getting-started/. Luettu: 1.11.2026.

Karvinen, T. 2025. Network Interface Names on Linux. Luettavissa: https://terokarvinen.com/network-interface-linux/. Luettu: 1.11.2026.
