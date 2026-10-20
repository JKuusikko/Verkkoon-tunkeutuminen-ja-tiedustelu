Hello world! 

## a) Hei maailma. Sieppaa liikennettä WireSharkilla. Merkitse TCP/IP-pinon kerrokset ruutukaappaukseen.

Tehtävän tein MacBook Pro m2:lla, jossa Tahoe 26.3.1. Koneessa on Virtualbox 7.2.7 arm versio ja Kali-Linux arm-64.

Aloitin tehtävän avaamalla wiresharkin terminaalista komennolla `wireshark` ja se avautui. Toisessa terminaalissa tein komennon: `curl http://example.com` ja wireshark päivittyi. Sammutin sen punaisesta näppäimestä ja suodatin liikennettä laittamalla kenttään `http`


<img width="1214" height="938" alt="image" src="https://github.com/user-attachments/assets/1b21da67-1457-4f7d-a714-d4348d2b5cc6" />

Kuvassa näkyy neljä eri kerrosta:

* Ethernet II → Peruskerros (linkkikerros)

* Internet Protocol Version 4 → Internet-kerros IP 10.0.2.15 -> 104.20.23.154

* Transmission Control Protocol → Kuljetuskerros TCP, kohdeportti 80

* Hypertext Transfer Protocol → Sovelluskerros HTTP GET /

Siellä näkyy myös kerros Frame 14, mutta se on netin perusteella joku Wiresharkin metatieto

## References:

- Karvinen, T. 2026: Create a Web Page Using Github. https://terokarvinen.com/2023/create-a-web-page-using-github/
- Wireshark User's Guide. https://www.wireshark.org/docs/
