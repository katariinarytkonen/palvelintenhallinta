### x) Lue ja tiivistä: Vapaavalintainen aiemman vuoden kotitehtäväraportti Saltin käytöstä Windowsilla. Löydät raportteja esimerkiksi Google tai Duck-haulla: salt windows karvinen.

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


### e) Komennus. Tee Salt-tila, joka asentaa järjestelmään uuden komennon.

### Lähteet:

