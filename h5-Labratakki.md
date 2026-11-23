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

## b) ICMP Spoofing ja TCP Session Hijacking. Aja molemmat labrat läpi ja kerro, miten molemmat tekniikat toimivat.

### ICMP Spoofing

Tehtävän aloitin samalla tavalla kuin edllesisessä kohdassa eli avasin ssh `ssh -Y mininet@192.168.2.130`, sitten tarkistin, että X11 on kunnossa istunnossa `echo $DISPLAY`, lisäsin X11 avaimen rootille `xauth list | while read line; do sudo xauth add $line; done`, siivosin vanhan Mininetin ja käynnistin uuden `sudo mn -c`,
`cd ~/lab/01-Network-Security-Lab`, `ryu-manager ryu.app.simple_switch_13 &`, `sudo mn --topo single,3 --mac --switch ovsk --controller remote` ja viimeisenä avasin xtermit h1 h2 ja h3 `mininet> xterm h1 h2 h3`:

<img width="2996" height="1094" alt="kuva" src="https://github.com/user-attachments/assets/d5aeea70-efd9-49e5-9a87-44f889f22340" />

Sitten aloitin labran ja ensin h3 ikkunassa eli hyökkääjän koneella tein komennot: `cd ~/lab/01-Network-Security-Lab/scripts` ja `python3 spoof_icmp.py`, joka jäi kuuntelemaan ICMP-liikennettä. Uhrilla (h1) käynnistin monitorin taustalle komennolla python3 sniff_icmp.py & ja pingasin sitten h2:ta komennolla ping -c 4 10.0.0.2.

<img width="966" height="682" alt="kuva" src="https://github.com/user-attachments/assets/6979759b-5bdb-44c3-b78a-1893e9116087" />

Monitorista näkyy, että ICMP-liikenne on kaapattavissa ja luettavissa verkossa. Varsinaisen väärennetyn vastauksen erottaminen oli Mininet-ympäristössä hankalaa, koska oikea kohde (h2) ehti vastata pingiin ennen hyökkääjää.

### Miten ICMP-spoofing toimii

ICMP (esimerkiksi ping) ei sisällä minkäänlaista todennusta eli kuka tahansa verkossa voi lähettää ICMP-vastauksen ja vastaanottaja luottaa siihen. Hyökkääjä voi käyttää tätä hyväkseen esimerkiksi teeskennelläkseen, että jokin kone on online tai osana man-in-the-middle-hyökkäystä ohjatakseen ja väärentääkseen liikennettä.

## TCP Session Hijacking

TCP Session Hijacking -labrassa hyökkääjä kaappaa aktiivisen TCP-istunnon injektoimalla siihen väärennetyn paketin. Aloitin labran ajamalla ensin ARP-myrkytyksen `arp_poison.py`, joka ohjasi liikenteen h3:n kautta. Sen jälkeen käynnistin `sniff_tcp_session.py`:n h3:lla. Sitten käynnistin palvelimen h1:llä `tcp_server.py` ja asiakkaan h2:lla `tcp_client.py`. Nyt h3:n sniffer näytti istunnon kaikki paketit SEQ- ja ACK-numeroineen. Poimin näistä viimeisimmän paketin numerot (SEQ 2171931357, ACK 2800773717) ja lähdeportin (56208).

<img width="2862" height="656" alt="kuva" src="https://github.com/user-attachments/assets/ca0b6018-fbf1-4ddd-ba2a-16bfe9f89023" />

Muokkasin tcp_hijack.py-skriptiin nämä napatut arvot ja ajoin sen. Skripti lähetti väärennetyn TCP-paketin, joka teeskenteli olevansa h2 (10.0.0.2:56208) ja injektoi h1:lle payloadin "you are hacked!".

<img width="2854" height="674" alt="kuva" src="https://github.com/user-attachments/assets/cd4304a6-525c-45dc-989d-a77df19200cf" />

### Miten TCP Session Hijacking toimii

TCP-istunto tunnistetaan IP-osoitteiden, porttien ja erityisesti sequence- ja acknowledgment-numeroiden perusteella. Jos hyökkääjä pääsee näkemään nämä numerot, hän voi rakentaa paketin oikeilla numeroilla ja injektoida sen istuntoon. Näin hyökkääjä voi lähettää omia komentojaan tai dataa aktiiviseen yhteyteen, vaikkei hän alun perin ollut osa sitä.

## c) 02-SDN-DDos_Simulation tryout-kansiossa on työkalut, jotta voit ajaa TCP SYN-Flood-hyökkäyksen turvallisesti. Kirjoita, miten ajoit hyökkäyksen ja miten kyseinen hyökkäys toimii

TCP SYN-Flood on palvelunestohyökkäys (DoS), jossa hyökkääjä lähettää suuren määrän TCP-yhteyden aloituspaketteja (SYN) viimeistelemättä kättelyä. Palvelin varaa resursseja jokaiselle keskeneräiselle yhteydelle ja jää odottamaan ACK-vastausta, joka ei tule. Lopulta palvelimen yhteystaulu täyttyy, eikä se pysty palvelemaan oikeita käyttäjiä.

Ajoin hyökkäyksen kansiossa 02-SDN_DDoS_Simulation-tryout. Varmistin ensin, että työkalu hping3 on asennettu `which hping3` -> /usr/sbin/hping3) eli asennettu. Käynnistin Ryu-ohjaimen komennolla `ryu-manager simple_switch_13.py &` ja ajoin sitten simulaation komennolla `sudo python3 tree_topology.py`.

<img width="1442" height="408" alt="kuva" src="https://github.com/user-attachments/assets/31deb23e-c9e5-4d51-ba28-cd00126c7f29" />

### Miten SYN-Flood toimii

TCP-yhteys alkaa kolmivaiheisella kättelyllä **SYN -> SYN-ACK -> ACK**. SYN-Flood-hyökkäyksessä hyökkääjä lähettää paljon SYN-paketteja muttei vastaa SYN-ACK:iin. Palvelin varaa resursseja keskeneräisille yhteyksille ja lopulta yhteystaulu täyttyy, josta seuraa palvelunesto. Väärennetty lähde-IP (spoofing) tekee hyökkääjän jäljittämisestä vaikeaa ja estää palvelinta sulkemasta yhteyksiä järkevästi.

## d) Vapaaehtoinen tutustu myös seuraaviin työkaluihin: EviNginx, PTunnel-ng. Kerro kyseisistä työkaluista, mitä ne tekevät, saitko asennettua ne, lisää ohjeraporttiin ja olivatko kyseiset työkalut mielenkiintoisia, jos olivat, niin miksi?

Teen vielä myöhemmin, jos aikaa.


## Lähteet

Ajith, S. ym. SDN_DDoS_Simulation. GitHub. Luettavissa: https://github.com/santhisenan/SDN_DDoS_Simulation. Luettu: 23.11.2026.

Anthropic. 2026. Claude. Käytetty apuna hyökkäysten selventämisessä sekä ongelmien selvittämisessä. Keskustelut: 23.11.2026.

Iso-Anttila, L. 2026. Mininet - luentomateriaali ja ohjeet (03-mininet.pdf). Haaga-Helia ammattikorkeakoulu. Luettavissa: https://hhmoodle.haaga-helia.fi/pluginfile.php/5055842/mod_resource/content/1/03-mininet.pdf. Luettu: 23.11.2026.

Karvinen, T. 2026. Verkkoon tunkeutuminen ja tiedustelu (tehtävänanto). Luettavissa: https://terokarvinen.com/verkkoon-tunkeutuminen-ja-tiedustelu/. Luettu: 23.11.2026.
