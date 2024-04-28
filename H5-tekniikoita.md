Ao. vastaukset ovat osa kotitehtävää 5 Tekniikoita Haaga-Helian Palvelinten Hallinta-kurssilla, jota opettaa Tero Karvinen. Tein harjoituksia 26.4. ja 28.4. kotona omalla tietokoneella Lenovo Yoga Slim 7 14ARE05. Hyödynsin osassa tehtäviä myös  virtuaalikonetta(Debian 12 bookworm) ja Virtualbox-alustaa. 

Harjoituksen tehtävänannot löytyvät osoitteesta : https://terokarvinen.com/2024/configuration-management-2024-spring/

### x) Lue ja tiivistä: Vapaavalintainen aiemman vuoden kotitehtäväraportti Saltin käytöstä Windowsilla. Löydät raportteja esimerkiksi Google tai Duck-haulla: salt windows karvinen.

Löysin vuoden 2019 raportin, jossa silloinen opiskelija Toni Seppä oli kuvannut kotitehtäväraportissaan Saltin käyttöä Windowsilla (https://salthomework.wordpress.com/h5/) . 
Hän oli testannut Windows-ympäristössäni Saltia ping-komennolla(./salt-call –local test.ping) ja käyttänyt myös komentoa ” salt-call –local system.set_computer_desc ‘tämä on uusi salt-minion windows 10 orja’ jolla oli muuttanut tietokoneen kuvausta tekstiä vastaavaksi. 

### a) Asenna Salt Windowsille tai Macille. Osoita 'salt-call --local' komentoa ajamalla, että asennus on onnistunut. (Jos olet asentanut jo aiemmin, tässä riittää pelkkä asennuksen testaaminen, eikä asennusta tarvitse tehdä uudelleen.)

    salt-call --local
    
Olin asentanut Saltin jo aiemmin windowsille, joten tässä komennon salt-call –local tulos, josta näkyy että Saltin asennus on onnistunut:

![image](https://github.com/katariinarytkonen/palvelintenhallinta/assets/164856665/b7ebcaba-7f07-46b0-9613-83a249c65475)


### b) Kerää Windows- tai Mac-koneesta tietoa grains.items -toiminnolla. Poimi 'grains.item' perään muutamia keskeisiä tietoja ja analysoi ne, eli selitä perusteellisesti mitä ne ovat. Kuvaile ja vertaile numeroita.

    salt-call –local grains.items | more
    salt-call –local grains.item osfinger
    salt-call –local grains.item cpu_model


Komennolle salt-call –local –grains sain näkyviin koneen tiedot. Koska olin tottunut käyttämään lähinnä Linuxin komentokehotetta, tuli minulla hieman ongelmia miten saan Windowsin komentokehoitteessa tiedot näkyvään ikkuna kerrallaan, eikä salt-call –local –grains | less toiminutkaan, jouduin tähän etsimään tietoa netistä. Komento

     salt-call –local grains.items | more

kuitenkin toimi, ja sain tiedot näkyviin toivotusti.

![image](https://github.com/katariinarytkonen/palvelintenhallinta/assets/164856665/10e16f7d-d145-45c2-b989-0c285c800009)

Katsoin kaikki tiedot, ja valitsin täältä joitain nostoja.

![image](https://github.com/katariinarytkonen/palvelintenhallinta/assets/164856665/e0c0dba8-a5c5-4257-b858-4ceda04617ec)

Näistä näen että prosessorini on AMD Ryzen 7 4700U

![image](https://github.com/katariinarytkonen/palvelintenhallinta/assets/164856665/0812e63d-d8fb-4bcd-a5d7-3e4bb8f0a3c7)

Tästä näkyy, että käyttöjärjestelmäni on Windows 10, vielä tarkemmin home-versio. 

![image](https://github.com/katariinarytkonen/palvelintenhallinta/assets/164856665/2cd7516c-4ea8-4b30-a67a-87c1eab5209d)

Koneeni on UTC+02:00 aikavyöhykkeellä.

### c) Kokeile Saltin file -toimintoa Windowsilla tai Macilla.

Kokeilin Saltin file-toimintoa Windowsilla file.managed-komennolla luomalla windowsin Temp-kansioon hellokatariina tiedoston. 
Tässä kohtaa piti etsiä netistä Windowsin Temp-tiedostojen polku, sillä tämä erosi Linuxin vastaavasta polusta, eikä salt-call --local state.single file.managed /tmp/hellokatariina tuonut toivottua tulosta:

![image](https://github.com/katariinarytkonen/palvelintenhallinta/assets/164856665/9ddad7bc-9d80-4029-b1c4-935dbc7064ef)

    salt-call --local state.single file.managed C:\WINDOWS\Temp/hellokatariina

Ensiksi tällä löytyi jo oppitunnilla luomani tiedosto, joten annoin vielä hieman erilaisen komennon(hellokatariina1) jotta näkyi myös jotain muuttuneen:

![image](https://github.com/katariinarytkonen/palvelintenhallinta/assets/164856665/8edea25d-1e98-4fb1-b1b1-eaabcadddcc1)


### d) CSI Kerava. Näytä 'find' avulla viimeisimmäksi muokatut tiedostot /etc/-hakemistosta ja kotihakemistostasi. Selitä kaikki käyttämäsi parametrit ja format string 'man find' avulla.

Tässä kohtaa käynnistin virtuaalikoneen vagrant upilla, ja otin siihen ssh-yhteyden tehtävän tekoa varten, vagrant ssh. Aloin tutkimaan find-komennon manuaalia 
    find man
Tämähän oli manuaalien tapaan aika valtava sisällöltään. Kurssin tehtäväsivulta löytyykin onneksi vinkki komennosta, jolla sain halutun tiedon ulos.

    find -printf '%T+ %p\n'|sort

Tässä find on komento, -printf tekee kommenosta tulosteen, %T+ saa näkyviin muokkausajan, %p tiedoston nimi, sort järjestää tulosteen halutusti ja /n on rivinvaihto.

Siirryin /etc/-hakemistoon, ja annoin komennon, josta sain ao. tulosteen:

![image](https://github.com/katariinarytkonen/palvelintenhallinta/assets/164856665/1f64d801-1867-4739-8f8d-5e6739a0b8b4)

Vaihdoin kotihakemistoon, cd ~,  ja annoin komennon uudelleen:

![image](https://github.com/katariinarytkonen/palvelintenhallinta/assets/164856665/019468ef-b823-43ee-a205-91f81d30c798)


### e) Komennus. Tee Salt-tila, joka asentaa järjestelmään uuden komennon.

Tässä aloitin uuden komennon lisäämisestä koneelle, jotta myöhemmin voin tehdä Salt-tilan. Loin skriptin moi.sh menemällä micro-tekstieditoriin, ja lisäämälle sinne skriptin

    micro moi.sh
   
    #!/usr/bin/bash
    echo "Moi!"

![image](https://github.com/katariinarytkonen/palvelintenhallinta/assets/164856665/7949d07f-10f1-4656-9e3e-3bb6f4ab9f8b)

Annoin skriptille suoritusoikeudet chmod komennolla, ja ajoin sen nähdäkseni toimiiko odotetusti.

    chmod ugo+x moi.sh
    ./moi.sh

![image](https://github.com/katariinarytkonen/palvelintenhallinta/assets/164856665/7e13daa8-c3b2-4b51-a4a1-c60ad378cd79)

Skripti toimi tässä. Seuraavaksi skripti piti siirtää /usr/local/bin-hakemistoon. Tein tämän cp-komennolla, johon vaadittiin sudo-oikeudet.

    sudo cp moi.sh /usr/local/bin

![image](https://github.com/katariinarytkonen/palvelintenhallinta/assets/164856665/958157fe-1f69-4b98-b003-b575745f03ab)

Testasin vielä että skripti toimi myös /usr/local/bin-tehtävässä ja ilman polkua.

![image](https://github.com/katariinarytkonen/palvelintenhallinta/assets/164856665/8037ad94-f284-4028-817c-3b4436d542ab)

Tämä toimi. Seuraavaksi tästä täytyi saada Salt-tila. Siirryin /srv/salt-kansioon, jossa Salt-tilat ovat, ja tein uuden directoryn moi.

![image](https://github.com/katariinarytkonen/palvelintenhallinta/assets/164856665/2252c275-8c97-40ab-881f-59d6055477d4)

Tässä kohtaa jäin jumiin ja yritin löytää googlesta apua, ja lopulta päädyin toisen opiskelijan kotitehtäväraporttiin (https://github.com/juliusjantti/palvelinten_hallinta_kurssi/blob/main/h5_CSI-kerava.md ), josta sain apua. 

Tästä näin, että minun täytyi luoda init.sls-tiedosto seuraavanlaisella sisällöllä tuohon /srv/salt-kansioon:

![image](https://github.com/katariinarytkonen/palvelintenhallinta/assets/164856665/ffe19a54-89b4-4a04-9ee7-90ae62975643)

Sitten täytyi luoda oikea skripti micro moi.sh, johon tuli ao. sisältö:

![image](https://github.com/katariinarytkonen/palvelintenhallinta/assets/164856665/52c44013-55e0-455c-bd9d-dacd2bdd586d)

Ajoin komennon, , ja tämä näytti toimineen.

     sudo salt-call –local state.apply moi

    
![image](https://github.com/katariinarytkonen/palvelintenhallinta/assets/164856665/59bb21e0-8eb4-4d41-89cb-e82371b0fe43)

     


### Lähteet:

https://stackoverflow.com/questions/1078920/equivalent-of-more-or-less-command-in-powershell Luettu 26.4.2024

https://learn.microsoft.com/en-us/answers/questions/1142543/is-it-safe-to-delete-temp-files-under-c-windowstem Luettu 28.4 

Karvinen 2024, Configuration Management 2024 spring. Luettavissa: 

https://terokarvinen.com/2024/configuration-management-2024-spring/ Luettu 26.4.2024

Seppä 2019, H5. Luettavissa:

https://salthomework.wordpress.com/h5/ Luettu 28.4.2024

Jäntti 2023, h5 CSI kerava. Luettavissa:

https://github.com/juliusjantti/palvelinten_hallinta_kurssi/blob/main/h5_CSI-kerava.md Luettu 28.4.2024

