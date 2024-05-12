Seuraavana vuorossa on kurssin lopputyö, oma Salt-miniprojektini.

Projektin tekijänä on Katariina Rytkönen ja projektini lisenssinä on GNU General Public License, versio 3.

Aloitan projektin luomalla virtuaaliympäristön, eli kaksi konetta Vagrantilla Teron ohjeilla. (https://terokarvinen.com/2021/two-machine-virtual-network-with-debian-11-bullseye-and-vagrant/?fromSearch=two%20machine%20virtual)

    mkdir twohost
    cd twohost
    nano Vagrantfile
    vagrant up
    vagrant ssh t001

![image](https://github.com/katariinarytkonen/palvelintenhallinta/assets/164856665/124a55ad-9592-4e33-bb87-fdce9f8e189a)

Seuraavaksi asensin koneille herra-orja-arkkitehtuurin niin, että t001 on herra, ja t002 orja.





### Lähteet

Karvinen 2024: Configuration management 2024 spring. Luettavissa:

https://terokarvinen.com/2024/configuration-management-2024-spring/. Luettu 12.5.2024

Karvinen Tero, 2021: Two Machine Virtual Network With Debian 11 Bullseye and Vagrant. Luettavissa:

https://terokarvinen.com/2021/two-machine-virtual-network-with-debian-11-bullseye-and-vagrant/ Luettu 12.5.2024

https://github.com/katariinarytkonen/palvelintenhallinta/blob/main/H2%20Soitto%20Kotiin.md Luettu 12.5.2024
