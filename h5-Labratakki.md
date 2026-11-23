## Laitteisto

Tehtävät tein MacBook pro 2019, jossa macOS Tahoe 26.6. Virtual Machinena toimi VMware Fusion 26.0.0 ja distrona Kali Linux amd64 sekä mininet-2.3.0.

## a) Aja tunnilla esitetty ARP hyökkäys ja tutki, miten se toimii.

Latasin mininetin koneelle jo ennen tuntia Larin ohjeiden mukaan eli latasin moodlen kautta:

<img width="1602" height="1252" alt="kuva" src="https://github.com/user-attachments/assets/876882cc-9685-4549-bdfc-8eb5aa0ccef4" />

Ensiksi selvitin VM:ip osoitteen komennolla `ip a`:

<img width="1750" height="364" alt="kuva" src="https://github.com/user-attachments/assets/8aec8840-4a05-45ed-9aed-a610d52319e1" />

Sitten tarkistin SSH-palvelun tilan komennolla `sudo systemctl status ssh`:

<img width="1600" height="762" alt="kuva" src="https://github.com/user-attachments/assets/360a7472-9e2f-447c-9a44-ea0d7d73bd2b" />

Sitten yhdistin Macin terminalilla Mininetiin:

<img width="1058" height="462" alt="kuva" src="https://github.com/user-attachments/assets/d92b5cb1-3ad5-4f46-bcd9-513eb1710727" />

Ajoin Macin konsolissa komennon `xterm`ja sovellus avautui:

<img width="2016" height="1286" alt="kuva" src="https://github.com/user-attachments/assets/61822b4e-e4d7-46de-a182-71753f6ff5c7" />

sitten ajoin komennot: `cd ~/lab/01-Network-Security-Lab`, `ryu-manager ryu.app.simple_switch_13 & ja `sudo mn --topo single,3 --mac --switch ovsk --controller remote`:

<img width="1704" height="796" alt="kuva" src="https://github.com/user-attachments/assets/718951bb-663a-4e44-8d4d-a64cb9a18093" />

Odotin, että pääsin taas kirjoittaa ja kokeilin komennoilla `mininet> pingall`,
`mininet> xterm h1` ja `mininet> xterm h3`.

<img width="2034" height="1362" alt="kuva" src="https://github.com/user-attachments/assets/89e2cc98-b1ec-4c88-8770-9d895b53349b" />

<img width="962" height="364" alt="kuva" src="https://github.com/user-attachments/assets/cb96a4bf-904a-4b2d-b061-3666f12c58e8" />

<img width="966" height="224" alt="kuva" src="https://github.com/user-attachments/assets/cc9de8b5-d471-41c1-935d-74d5143e9886" />

<img width="1954" height="692" alt="kuva" src="https://github.com/user-attachments/assets/e209e5fa-1080-485c-8a12-986f19cb76cb" />





