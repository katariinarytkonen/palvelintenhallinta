Ao. vastaukset ovat osa kotitehtävää H2 Soitto kotiin Haaga-Helian Palvelinten Hallinta-kurssilla, jota opettaa Tero Karvinen. Harjoituksia tehtiin 5.4. ja 7.4. kotona omalla tietokoneella Lenovo Yoga Slim 7 14ARE05. Hyödynsin tehtävissä virtuaalikoneita ja Virtualbox-alustaa. Kirjoitin joihinkin tehtäviin tehtävänannon jälkeen olennaiset komennot, jotta ne löytyvät helposti.

Harjoitusten tehtävänannot löytyvät osoitteesta :
https://terokarvinen.com/2024/configuration-management-2024-spring/


#### Lue ja Tiivistä

#### a) Asenna kaksi virtuaalikonetta samaan verkkoon. Osoita, että pystyt käyttämään kumpaakin konetta (esim 'vagrant ssh t001'). Osoita, että koneet voivat pingata toisiaan. (Tämä tehtävä on helpointa tehdä Vagrantilla)
    mkdir 

Alkuun luon uuden directoryn twohost, johon tallennan ao. tekstin nimellä Vagrantfile. Tiedosto sisältää tarvittavat konfiguraatiot, jotta kaksi virtuaalikonetta, t001 ja t002 asentuvat oikein. Laitan virtuaalikoneet päälle

![image](https://github.com/katariinarytkonen/ICI001AS3A-3005/assets/164856665/669315b4-32a4-4b5f-be33-5af7f9178750)
![image](https://github.com/katariinarytkonen/ICI001AS3A-3005/assets/164856665/1e2aba6d-2861-4a8a-a721-fe3e059c8e31)

Nyt virtuaalikoneeni t001 ja t002 ovat pystyssä, ja voin ottaa näihin etäyhteyden komennolla vagrant ssh t001 ja vagrant ssh t002. Testaan että molemmat toimivat, ja poistun etäyhteyksistä exit komennolla.
![image](https://github.com/katariinarytkonen/ICI001AS3A-3005/assets/164856665/b8660582-f324-4181-bc34-0fde81eefd83)

Testaan myös, että koneet voivat pingata toisiaan selvittämällä molempien koneiden ip-osoitteet ip a komennolla. Koneen t001 ip-osoite on 192.168.88.101 t001 ja t002 osoite 192.168.88.102.

![image](https://github.com/katariinarytkonen/ICI001AS3A-3005/assets/164856665/306c6878-aa67-425f-9238-238c524124ce)

![image](https://github.com/katariinarytkonen/ICI001AS3A-3005/assets/164856665/e35c0fea-c8df-44dd-ad88-e80d404109ce)

Kaikki toimii molempiin suuntiin, ja näin olen asentanut kaksi virtuaalikonetta samaan verkkoon, ja pystyin käyttämään kumpaakin ja koneet pystyvät löytämään toisensa.


####  b) Asenna Saltin herra-orja arkkitehtuuri toimimaan verkon yli. (Verkko voi olla virtuaalinen verkko paikallisten virtuaalikoneiden välillä, kuten muissakin alakohdissa)
        sudo apt-get update
        sudo apt-get -y install salt-master
        hostname -I
        sudoedit /etc/salt/minion 
        sudo systemctl restast salt-minion.service
        
Asensin Saltin herra-orja-arkkitehtuurin toimimaan verkon yli luomissani virtuaalikoneissa. Tätä varten t001 on herra, ja t002 on orja.

Asennan t001 herran komennolla sudo apt-get -y install salt-master.
Selvitän ip-osoitteen komennolla hostname -I, jota tarvitsen tulevaisuudessa. Poistun yhteydestä exit-komennolla asentaakseni toisesta koneesta orjan.
Komennolla sudoedit /etc/salt/minion menen lisäämään tiedostoon herran ip-osoitteen, jotta orja tunnistaa herran ja kaikki toimii kuten pitää.
Käynnistän demonin uudelleen ja vaihdan t002 koneelle hyväksyäkseni avaimen.

![image](https://github.com/katariinarytkonen/ICI001AS3A-3005/assets/164856665/513040c1-e311-43ad-92c1-d07300b17f34)

Koska jätin id:ni tyhjäksi, tuli nimeksi None.

        

#### c) Aja shell-komento orjalla Saltin master-slave yhteyden yli.
        sudo salt ’*’ cmd.run ’whoami’
Komennolla sudo salt ’*’ cmd.run ’whoami’ ajan shell-komennon orjalla Saltin master-slave yhteyden yli:
![image](https://github.com/katariinarytkonen/ICI001AS3A-3005/assets/164856665/26f5fc3d-1d38-4f17-804a-39592821115d)

    

#### d) Aja useita idempotentteja (state.single) komentoja master-slave yhteyden yli.
        sudo salt ’*’ -l info state.single cmd.run touch /tmp/testing’ creates=”/tmp/testing”
        sudo salt ’*’ -l info state.single user.present rytkokat
        sudo salt ’*’ -l info state.single user.absent rytkokat
        cat /etc/passwd
        sudo salt ‘*’ -l info state.single file.managed /tmp/testitiedosto


Jatkan tämän tehtävän parissa seuraavana päivänä, joten aloitin projektin laittamalla virtuaalikoneet päälle, ja ottamalla ssh-yhteyden herraan suorittaakseni idempotentteja komentoja orjalle. 
![image](https://github.com/katariinarytkonen/ICI001AS3A-3005/assets/164856665/8e6c87a7-4d0e-4cc2-bf49-2dba5e2db00f)

Ajan komennon vielä uudelleen esimerkkinä idempotenssista, joka oli jäänyt ensimmäiseltä oppitunnilla hieman itselleni epäselväksi. Nyt kuitenkin näen sen toimintaa tässä käytännössä, eli kun ajoin uudelleen saman käskyn, näyttää järjestelmä että käskyn tila oli onnistunut, mutta koska tämä oli jo olemassa, ei mitään muutettu tällä kertaa.
![image](https://github.com/katariinarytkonen/ICI001AS3A-3005/assets/164856665/f9be4a39-655b-47fd-9779-21477bf9df8d)

Komennolla user.present ja user.absent luon ja poistan käyttäjän rytkokat orjakoneelta. Luon käyttäjän rytkokat vielä uudelleen käydäkseni vilkaisemassa orjakoneelta idempotenssin toimintaa käytännössä, ja nähdäkseni kuinka luomani käyttäjä löytyy täältä. Tiedot käyttäjistä löytyy ainakin passwd-tiedostosta, josta rytkokat löytyikin.
![image](https://github.com/katariinarytkonen/ICI001AS3A-3005/assets/164856665/9e5a5f1e-3391-4728-ae0f-596beee2e713)

![image](https://github.com/katariinarytkonen/ICI001AS3A-3005/assets/164856665/c923c3b1-de8e-42d6-91ae-5cdf0ddb0b11)

![image](https://github.com/katariinarytkonen/ICI001AS3A-3005/assets/164856665/d53622da-f7aa-42fe-8d62-b035d78bfcdf)

Palasin takaisin idempotentteihin komentoihin, ja loin testitiedoston.

![image](https://github.com/katariinarytkonen/ICI001AS3A-3005/assets/164856665/293ceca5-5f30-49f3-8a7d-d510ba1ef3e8)







    
#### e) Kerää teknistä tietoa orjista verkon yli (grains.item)
        sudo salt '*' grains.items
        sudo salt '*' grains.item username
        sudo salt '*' grains.item osfinger
        sudo salt '*' grains.item virtual

    
Ylläolevilla komennoilla keräsin erinäistä teknistä tietoa orjista verkon yli. Komennoista käy ilmi orjan username, käyttöjärjestelmä sekä se, että orja toimii virtualboxin kautta virtuaalikoneena.

![image](https://github.com/katariinarytkonen/ICI001AS3A-3005/assets/164856665/be91e75a-6159-469d-bb52-10b2dac5f10f)

#### f) Hello, IaC. Tee infraa koodina kirjoittamalla /srv/salt/hello/init.sls. Aja tila jollekin orjalle. Tila voi esimerkiksi tehdä esimerkkitiedoston johonkin hakemistoon. Testaa toisella komennolla, että pyytämäsi muutos on todella tehty.

En osannut täysin tätä tehtävää. Löysin opettajan esimerkkisivun (https://terokarvinen.com/2018/salt-quickstart-salt-stack-master-and-slave-on-ubuntu-linux/), jonka mukaan sain jotain tehtyä, mutta en täysin ymmärtänyt mihin tehtävänannossa annettu koodi olisi pitänyt laittaa. 



#### Lähteet
Karvinen 2024: Configuration management 2024 spring. Luettavissa:
https://terokarvinen.com/2024/configuration-management-2024-spring/. Luettu 5.4.2024

Karvinen 2021: Two Machine Virtual Network With Debian 11 Bullseye and Vagrant. Luettavissa:

https://terokarvinen.com/2021/two-machine-virtual-network-with-debian-11-bullseye-and-vagrant/ Luettu 5.4.2024

Karvinen 2018: Salt Quickstart – Salt Stack Master and Slave on Ubuntu Linux. Luettavissa:

https://terokarvinen.com/2018/salt-quickstart-salt-stack-master-and-slave-on-ubuntu-linux/ Luettu 5.4.2024
 
Linux List Users: How to Display All Users in the Terminal?. Luettavissa:
https://www.hostinger.com/tutorials/how-to-see-system-users-in-ubuntu-linux-vps/#How_to_List_Users_in_Linux. Luettu 7.4.2024

Karvinen 2018: Salt States – I Want My Computers Like This. Luettavissa:
https://terokarvinen.com/2018/salt-states-i-want-my-computers-like-this/ Luettu: 7.4.2024

Karvinen 2024: Hello Salt Infra-as-Code. Luettavissa:
https://terokarvinen.com/2024/hello-salt-infra-as-code/ Luettu 7.4.2024

Karvinen 2018: Configure Windows and Linux with Single Salt Module. Luettavissa:
https://terokarvinen.com/2018/configure-windows-and-linux-with-salt-jinja-if-else-and-grains/ Luettu 7.4.2024


