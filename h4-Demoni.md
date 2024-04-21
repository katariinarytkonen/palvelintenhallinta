Ao. vastausket ovat osa kurssitehtävää H4 DemoniHaaga-Helian Palvelinten Hallinta-kurssilla, jota opettaa Tero Karvinen.
Tein harjoituksia 21.4.kotona omalla tietokoneella Lenovo Yoga Slim 7 14ARE05. Hyödynsin tehtävissä virtuaalikonetta(Debian 12 bookworm) ja Virtualbox-alustaa.
Harjoituksen tehtävänannot löytyy osoitteesta https://terokarvinen.com/2024/configuration-management-2024-spring/.

## X, Lue ja tiivistä
### Karvinen 2023: Salt Vagrant - automatically provision one master and two slaves, kohdat :
### -Infra as Code - Your wishes as a text file
### -top.sls - What Slave Runs What States

Infraa koodina tarkoittaa yksinkertaisimmillaan sitä, että kirjoitetaan toive tavoitetilasta tekstitiedoksi, ja ajetaan se. Tässä kielenä on YAML, jossa on tärkeää käyttää sisennyksissä kahta välilyöntiä, jotta kongifuraatiotiedostot toimivat. 
Top.sls-tiedostossa on pääasetukset ja tieto siitä, mitä tiloja mikäkin orja ajaa.  

## Salt contributors: Salt overview, kohdat:
### -Rules of YAML
### -YAML simple structure
### -Lists and dictionaries - YAML block structures

YAML on markup-kieli, joka yhdistää  YAMLin data structuren Pythonin datastructureen Saltia varten. 

YAMLin muutami erityispiirteitä on:
- data on muotoiltu?(structured) key:value pareissa
- kirjainkoolla on väliä
- tabia ei saa käyttää, vain välilyönnit sallitaan
- kommentit risuaidalla # 

## Karvinen 2018: Pkg-File-Service – Control Daemons with Salt – Change SSH Server Port

## Silmäile Saltin ohjeet tilafunktioille pkg, file ja service. 

## a) Hello SLS! Tee Hei maailma -tila kirjoittamalla se tekstitiedostoon, esim /srv/salt/hello/init.sls.

    sudo mkdir -p /srv/salt/hello/
    sudoedit init.sls
  
Käytin tehtävässä jo aiemmin luomaani virtuaalikonetta, jossa oli salt ja micro jo asennettuina.
Loin uuden directoryn /srv/salt/hello, johon loin hello-moduulin.
Muokkasin init.sls-tiedostoa lisäämällä tänne seuraavan funktion, joka loi /tmp/kansioon hellokatariina tiedoston:

    /tmp/hellokatariina
      file.managed

![image](https://github.com/katariinarytkonen/palvelintenhallinta/assets/164856665/b349132d-dc99-40a9-bdad-6b587eaaafc1)

Kävin tarkastamassa, että se loi mitä pitikin.

![image](https://github.com/katariinarytkonen/palvelintenhallinta/assets/164856665/196782e0-233a-4e30-8337-a1b782211bb2)

## b) Top. Tee top.sls niin, että useita valitsemiasi tiloja ajetaan automaattisesti, esim komennolla "sudo salt '*' state.apply" tai "sudo salt-call --local state.apply".

Aloitin siirtymällä /srv/salt, ja loin sinne top.sls-tiedoston seuraavilla tiedoilla:

![image](https://github.com/katariinarytkonen/palvelintenhallinta/assets/164856665/6404ad3a-f4a0-43c3-808d-aad1899c333f)

Hello on edellisessä tehtävässä luomani moduuli, ja teen vielä toisen, favourites-moduulin jolla asensin erinäisiä ohjelmia.

![image](https://github.com/katariinarytkonen/palvelintenhallinta/assets/164856665/36563782-93c3-4da2-9989-57b5cb50724a)

Sitten yritin ajaa tätä ao. komennolla, mutta virheitä tuli:

    sudo salt-call --local state.apply

    ![image](https://github.com/katariinarytkonen/palvelintenhallinta/assets/164856665/5fed0b80-2c51-4550-9842-3d57a07fd24e)

top.sls-tiedostossani näytti olevan virhe, muokkasin hieman sisennyksiä ja uusi yritys.

Se ei kuitenkaan auttanut, nyt kävin muokkaamassa hello-moduuliani johon olin lisännyt ylimääräistä materiaalia, ja sain lopulta jonkin tuloksen, en tosin aivan sitä minkä halusin.

![image](https://github.com/katariinarytkonen/palvelintenhallinta/assets/164856665/3984eaf2-f717-4903-97a3-ce4dde9d28dc)

Seuraavaksi kävin muokkaamassa favourites-moduulin sisältöä, mutta ei tämä nyt toiminut aivan niinkuin ajattelin.

## c) Apache easy mode. Asenna Apache, korvaa sen testisivu ja varmista, että demoni käynnistyy.

Ensiksi asensin apachen ja curlin käsin komennoilla

    sudo apt-get install apache2
    sudo apt-get install curl

Testasin että apache toimii:

    wget https://localhost
    
Katsoin silloista index-sivua 

    less index.html
    curl -s localhost | grep title

   ![image](https://github.com/katariinarytkonen/palvelintenhallinta/assets/164856665/133f16f8-223c-4fbc-949c-dc9bff434a10)

Seuraavaksi tämä piti korvata uudella sivulla, johon muokkasin oman etusivuni

    micro index.html
    
![image](https://github.com/katariinarytkonen/palvelintenhallinta/assets/164856665/9c30a9c9-800d-472b-81d2-aa62e21bed38)

    cd /etc/apache2

Muokkasin tänne conf-tiedostoa, ja laitoin tähän linkin luomaani testisivuun:

    micro katariina.conf
    sudo rm 0000.default.conf
    
Poistin sites-enabledista siinä olevan defaultsivun, ja loin symbolisen linkin omaan katariina.conf-tiedostooni

    sudo ln -s /sites-available/katariina.conf

Yritin restartata demonia:

    sudo systemctl restart-apache2

Jassoo, ei toiminutkaan:

![image](https://github.com/katariinarytkonen/palvelintenhallinta/assets/164856665/271232d6-00af-4ad5-8083-571c17216519)

Katsoin palvelimen statusta

    sudo systemctl status apache2

 ![image](https://github.com/katariinarytkonen/palvelintenhallinta/assets/164856665/71d5a5c7-6ec4-498d-811d-83482e32987a)

Näytti siltä että linkissäni saattoi olla joku virhe, ja testisivu mihin yritti ohjata, olikin tyhjä.

Kävin muokkaamassa sites-availablessa polkua, josta tämä haki index-sivuston.

![image](https://github.com/katariinarytkonen/palvelintenhallinta/assets/164856665/77a59603-3cc5-4898-8da1-1266eadcaee8)

Ei auttanut vieläkään. Uusi yritys myöhemmin.





## d) SSHouto. Lisää uusi portti, jossa SSHd kuuntelee.


## Lähteet

Karvinen 2023: Salt Vagrant - automatically provision one master and two slaves. Luettavissa:
https://terokarvinen.com/2023/salt-vagrant/#infra-as-code---your-wishes-as-a-text-file. Luettu 21.4.2024

https://docs.saltproject.io/salt/user-guide/en/latest/topics/overview.html#rules-of-yaml Luettu: 21.4.2024

Karvinen 2024: Configuration Management 2024 Spring. Luettavissa:
https://terokarvinen.com/2024/configuration-management-2024-spring/ Luettu: 21.4.2024

Karvinen 2018: Pkg-File-Service – Control Daemons with Salt – Change SSH Server Port. Luettavissa:
https://terokarvinen.com/2018/04/03/pkg-file-service-control-daemons-with-salt-change-ssh-server-port/?fromSearch=karvinen%20salt%20ssh. Luettu 21.4.2024
