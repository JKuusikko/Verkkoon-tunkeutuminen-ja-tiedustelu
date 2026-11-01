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




## Lähteet

Karvinen, T.
