Ao. vastaukset ovat osa kotitehtävää H2 Soitto kotiin Haaga-Helian Palvelinten Hallinta-kurssilla, jota opettaa Tero Karvinen. Harjoituksia tehtiin 5.4. ja 7.4. kotona omalla tietokoneella Lenovo Yoga Slim 7 14ARE05. Hyödynsin tehtävissä virtuaalikoneita ja Virtualbox-alustaa. 

Harjoitusten tehtävänannot löytyvät osoitteesta :
https://terokarvinen.com/2024/configuration-management-2024-spring/


#### Lue ja Tiivistä

#### a) Asenna kaksi virtuaalikonetta samaan verkkoon. Osoita, että pystyt käyttämään kumpaakin konetta (esim 'vagrant ssh t001'). Osoita, että koneet voivat pingata toisiaan. (Tämä tehtävä on helpointa tehdä Vagrantilla)
    mkdir 

#### d) Aja useita idempotentteja (state.single) komentoja master-slave yhteyden yli.

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



#### Lähteet
Karvinen 2024: Configuration management 2024 spring. Luettavissa:
https://terokarvinen.com/2024/configuration-management-2024-spring/. Luettu 5.4.2024

Karvinen 2021: Two Machine Virtual Network With Debian 11 Bullseye and Vagrant. Luettavissa:

https://terokarvinen.com/2021/two-machine-virtual-network-with-debian-11-bullseye-and-vagrant/ Luettu 5.4.2024

Karvinen 2018: Salt Quickstart – Salt Stack Master and Slave on Ubuntu Linux. Luettavissa:

https://terokarvinen.com/2018/salt-quickstart-salt-stack-master-and-slave-on-ubuntu-linux/ Luettu 5.4.2024
 
Linux List Users: How to Display All Users in the Terminal?. Luettavissa:
https://www.hostinger.com/tutorials/how-to-see-system-users-in-ubuntu-linux-vps/#How_to_List_Users_in_Linux. Luettu 7.4.2024
