## Laitteisto

Tehtävät tein MacBook pro 2019, jossa macOS Tahoe 26.6. Virtual Machinena toimi VMware Fusion 26.0.0 ja mininet-2.3.0, Ubuntu 20.04.

## a) Aja tunnilla esitetty ARP hyökkäys ja tutki, miten se toimii.

Latasin mininetin koneelle jo ennen tuntia Larin ohjeiden mukaan eli latasin moodlen kautta valmiin Mininet-VM-imagen ja toin sen VMware Fusioniin:

<img width="1602" height="1252" alt="kuva" src="https://github.com/user-attachments/assets/876882cc-9685-4549-bdfc-8eb5aa0ccef4" />

Ensiksi selvitin VM:ip osoitteen komennolla `ip a`:

<img width="1750" height="364" alt="kuva" src="https://github.com/user-attachments/assets/8aec8840-4a05-45ed-9aed-a610d52319e1" />

Sitten tarkistin SSH-palvelun tilan komennolla `sudo systemctl status ssh`:

<img width="1600" height="762" alt="kuva" src="https://github.com/user-attachments/assets/360a7472-9e2f-447c-9a44-ea0d7d73bd2b" />

Sitten yhdistin Macin terminalilla Mininetiin, ennen tätä kuitenkin latasin Macille XQuartz ohjelman:

<img width="1058" height="462" alt="kuva" src="https://github.com/user-attachments/assets/d92b5cb1-3ad5-4f46-bcd9-513eb1710727" />

Ajoin Macin konsolissa komennon `xterm`ja sovellus avautui:

<img width="2016" height="1286" alt="kuva" src="https://github.com/user-attachments/assets/61822b4e-e4d7-46de-a182-71753f6ff5c7" />

sitten ajoin komennot: `cd ~/lab/01-Network-Security-Lab`, `ryu-manager ryu.app.simple_switch_13 &` ja `sudo mn --topo single,3 --mac --switch ovsk --controller remote`:

<img width="1704" height="796" alt="kuva" src="https://github.com/user-attachments/assets/718951bb-663a-4e44-8d4d-a64cb9a18093" />

Odotin, että pääsin taas kirjoittaa ja kokeilin komennoilla `mininet> pingall`,
`mininet> xterm h1` ja `mininet> xterm h3`. Ensin xtermit eivät auenneet, vaan tuli virhe "X11 connection rejected because of wrong authentication". Korjasin tämän lisäämällä X11-avaimen root-käyttäjälle, minkä jälkeen xterm h1 ja xterm h3 avautuivat omina ikkunoinaan Macin näytölle.

<img width="2034" height="1362" alt="kuva" src="https://github.com/user-attachments/assets/89e2cc98-b1ec-4c88-8770-9d895b53349b" />

h1-ikkunassa tarkistin ARP-taulun komennolla `arp -n`:

<img width="962" height="364" alt="kuva" src="https://github.com/user-attachments/assets/cb96a4bf-904a-4b2d-b061-3666f12c58e8" />

h3-ikkunassa ajoin seuraavat komennot: ensiksi otin käyttöön pakettien edelleenvälityksen `sysctl -w net.ipv4.ip_forward=1` ,sitten ohjasin SSH-liikenteen h3:n porttiin, käynnistin liikenteen kaappauksen `tcpdump -i h3-eth0 -w arp-p.pcap &` ja väärennetyn SSH-palvelimen `sshesame -port 22 -listen_address 0.0.0.0 &`. Lopuksi ajoin itse myrkytysskriptin `python3 arp_poison.py`. Tarkistin h1:n ARP-taulun uudelleen:

<img width="966" height="224" alt="kuva" src="https://github.com/user-attachments/assets/cc9de8b5-d471-41c1-935d-74d5143e9886" />

Testi: h1 yritti muodostaa SSH-yhteyden "h2:lle" (ssh h2). Liikenne ohjautui myrkytyksen takia h3:lle, jonka sshesame-palvelin sieppasi h1:n antaman salasanan selkokielisenä (password=salasana).

<img width="1954" height="692" alt="kuva" src="https://github.com/user-attachments/assets/e209e5fa-1080-485c-8a12-986f19cb76cb" />

Kuitenkin huomasin, että SSH itse varoitti host key -muutoksesta "REMOTE HOST IDENTIFICATION HAS CHANGED", mikä on nimenomaan merkki man-in-the-middle-hyökkäyksestä.

### Miten hyökkäy toimii

ARP muuntaa IP-osoitteen MAC-osoitteeksi lähiverkossa, eikä se vahvista vastauksia mitenkään. Hyökkääjä lähettää väärennettyjä ARP-vastauksia, jolloin uhrien ARP-välimuistit myrkyttyvät ja liikenne ohjautuu hyökkääjän kautta. Näin hyökkääjä pääsee kuuntelemaan ja muokkaamaan liikennettä (man-in-the-middle), kuten SSH-salasanan sieppaus tässä osoitti.


## Lähteet



Iso-Anttila, L. 2026. Mininet - luentomateriaali ja ohjeet (03-mininet.pdf). Haaga-Helia ammattikorkeakoulu. Luettavissa: https://hhmoodle.haaga-helia.fi/pluginfile.php/5055842/mod_resource/content/1/03-mininet.pdf. Luettu: 23.11.2026.

Karvinen, T. 2026. Verkkoon tunkeutuminen ja tiedustelu (tehtävänanto). Luettavissa: https://terokarvinen.com/verkkoon-tunkeutuminen-ja-tiedustelu/. Luettu: 23.11.2026.
