Ao. vastaukset ovat osa kotitehtävää H3 Toimiva versio Haaga-Helian Palvelinten Hallinta-kurssilla, jota opettaa Tero Karvinen. Tein harjoituksia 13.4. ja 14.4. kotona omalla tietokoneella Lenovo Yoga Slim 7 14ARE05. Hyödynsin tehtävissä virtuaalikonetta(Debian 12 bookworm) ja Virtualbox-alustaa. Kirjoitin joihinkin tehtäviin tehtävänannon jälkeen olennaiset komennot, jotta ne löytyvät myöhemminkin helposti.

Harjoituksen tehtävänannot löytyvät osoitteesta : https://terokarvinen.com/2024/configuration-management-2024-spring/

## x) Lue ja tiivistä. 
### Chacon and Straub 2014: Pro Git, 2ed: 1.3 Getting Started - What is Git? 
-

### Gitin käyttö on lähinnä 'git add . && git commit; git pull && git push'. Selitä tuon komennon jokainen osa. Käytä apuna itse valitsemiasi lähteitä ja viittaa niihin

- Git add .  Lisää jotain Gitiin
- Git Commit; Tallentaa työn anna commit-message
- git pull: vetää gitin serveriltä uusimmat tiedot
- git push: siirtää tiedot serverille

- && "If success, then continue". kaksi näitä peräkkäin tarkoittaa sitä, että mikäli käsky vasemmalla 

### Varaston terokarvinen/suolax/ historia, eli loki ja muutokset. Kätevimmin komentokehotteesta 'git clone https://github.com/terokarvinen/suolax.git; cd suolax/; git log --patch --color|less -R'. Wepistäkin saattaa onnistua kliksuttelemalla "Commits".


## a) Online. Tee uusi varasto GitHubiin (tai Gitlabiin tai mihin vain vastaavaan palveluun). Varaston nimessä ja lyhyessä kuvauksessa tulee olla sana "summer". Aiemmin tehty varasto ei kelpaa. 
 Luon uuden repositoryn GitHubiin graafisessa liittymässä, valitsen tälle nimen, lisään descriptionin ja README.filen ja lisenssiksi GNU General Public License 3.
 
 ![image](https://github.com/katariinarytkonen/palvelintenhallinta/assets/164856665/93ae00fa-1b92-4fd2-b49e-35c60120f979)

 ## b) Dolly. Kloonaa edellisessä kohdassa tehty uusi varasto itsellesi, tee muutoksia omalla koneella, puske ne palvelimelle, ja näytä, että ne ilmestyvät weppiliittymään.
    cd code
    git clone (+ url)
    ssh-keygen        
    micro.ssh
    git add . && git commit; git pull && git push
    

Aloitin tehtävän käynnistämällä virtuaalikoneen, Debianin, sillä totesin edellisellä oppitunnilla että Gitin käyttö on huomattavasti miellyttävämpää linuxilla kuin omalla windowsillani.
Alkuun siirryin cd code-komennolla edellisellä oppitunnilla luomaani directoryyn code, josta käsin tein päivityksiä githubiin kloonatakseni edellisessä kohdassa tehdyn uuden varaston, Summerin, itselleni, komennolla git clone plus luomani repositoryn linkki.
Tämä ei kuitenkaan toiminut, ja sain seuraavanlaisen virhe-ilmoituksen:

![image](https://github.com/katariinarytkonen/palvelintenhallinta/assets/164856665/d9abda15-70c7-4a76-b2e3-8e1e521bc679)

Tarkistin kansion sisällön, ja tajusin että tämä ei toiminut, koska olin linkittänyt edellisellä tunnilla vain erään toisen repositoryn tähän, ja tämä vaatii nyt uuden repositoryn siirron perusteellisemmin.
Peruutin luomastani kansiosta kotihakemistoon, annoin komennon uudelleen, ja siirryin tiedostoon.
Avazin micro-tekstinkäsittelyeditorilla tiedoston julkisen avaimen, joka on .pub-päätteinen, kopioin tämän, ja siirryin GitHubin graafiseen liittymään lisätäkseni avaimen sisällön sinne.

![image](https://github.com/katariinarytkonen/palvelintenhallinta/assets/164856665/253d96ee-734f-4b82-83ef-38e6e50122ef)

![image](https://github.com/katariinarytkonen/palvelintenhallinta/assets/164856665/63a374a4-7fed-4aed-a338-e0c69e4f9c7f)


Github herjasi että avain oli jo käytössä. Päädyin testaamaan alussa antamaani komentoa ssh clone kloonatakseni repositoryn uudestaan, ja nyt tällä kertaa tämä onnistuikin jostain tuntemattomasta syystä, ja Summer-reponi näkyykin terminaalin kautta.
Tämä ssh-avainten kanssa säätäminen oli hyvää kertausta alkuasennuksista, vaikka nyt tällä kertaa olikin turhaa!

![image](https://github.com/katariinarytkonen/palvelintenhallinta/assets/164856665/81c15b17-794c-4edd-ae13-5e945812bd06)

Seuraavaksi loin directoryyn uuden tiedoston Testi.md , ja ajoin tämän Githubiin. Lisäsin commit-messagen, ja tarkistin GUI:sta että tämä näkyi myös siellä.

![image](https://github.com/katariinarytkonen/palvelintenhallinta/assets/164856665/132d75ed-6e1b-4404-aadb-269195dcd1f3)

![image](https://github.com/katariinarytkonen/palvelintenhallinta/assets/164856665/871bb19b-fd7c-47a2-bdd4-b332e8db2caa)

Kaikki toimi, eli sain kloonattua edellisessä tehtävässä luomani repositoryn(varaston) itselleni, tein muutoksia omalla tietokoneellani, puskin ne palvelimelleni ja näytin että ne ilmestyivät web-liittymään.

## c) Doh! Tee tyhmä muutos gittiin, älä tee commit:tia. Tuhoa huonot muutokset ‘git reset --hard’. Huomaa, että tässä toiminnossa ei ole peruutusnappia.

    git add
    git reset --hard
    
Oletan tyhmän muutoksen tarkoittavan esim jotain tiedostoa, mitä ei ollut tarkoittanut laittaa julkiseksi, joten luon uuden tiedoston testirepositoryyn, lisään tämän gitiin git add-komennolla,  jonka poistankin ennen kuin ajan näitä GitHubiin. 

![image](https://github.com/katariinarytkonen/palvelintenhallinta/assets/164856665/28ab002f-2b33-4b49-930b-2e9a3664ef8f)

![image](https://github.com/katariinarytkonen/palvelintenhallinta/assets/164856665/05f5e549-0e39-488e-b3aa-4651a171d625)

Varmistan vielä että poistuihan väärä tiedostoni; poistui.
![image](https://github.com/katariinarytkonen/palvelintenhallinta/assets/164856665/d6e97f0d-faf9-48c3-bf05-e70f798d6aed)

## Tukki. Tarkastele ja selitä varastosi lokia. Tarkista, että nimesi ja sähköpostiosoitteesi näkyy haluamallasi tavalla ja korjaa tarvittaessa.

    git log –patch
    git config --global user.email ""
    git config --global user.name ""

Tarkistelen varastoni lokia komennolla komennolla git log --patch
![image](https://github.com/katariinarytkonen/palvelintenhallinta/assets/164856665/e2513ec6-193f-4aa4-b21b-f117a1a6fa07)

Heti alkuun huomasin että nimi on oikein, mutta sähköposti ei, joten korjasin nuo git config --global user.email komennolla. 
Tämän jälkeen tein muutoksia testitiedostooni saadakseni lokiin hieman enemmän sisältöä.
Ajoin git log-kommennon uudelleen, mutta havaitsin että muutokseni eikä muuttunut sähköposti näy lokissa. Testasin luoda ja lisätä uuden tiedoston, ja ajaa tämän serverille.

![image](https://github.com/katariinarytkonen/palvelintenhallinta/assets/164856665/fb0ab6d4-64c5-48eb-baa8-ce87b9c56570)

Nyt lokissa näkyivät päivityksetkin, joten saatoin unohtaa aiemmassa kohdassa ajaa päivitykset githubin serverille git pull & git pushilla.

Lokissa näkyy ensimmäisenä uusimmat päivitykset. Author-kohdasta näkee päivittäjän nimen ja sähköpostiosoitteen, sekä päivän ja ajan jolloin muutos on tehty. Tämän alla näkyy commit message, gitin koodia, ja vihreällä näkyy tehty muutettu teksti.

## e) Suolattu rakki. Aja Salt-tiloja omasta varastostasi. (Salt tiedostot mistä vain hakemistosta "--file-root teronSaltHakemisto". Esimerkiksi 'sudo salt-call --local --file-root srv/salt/ state.apply', huomaa suhteellinen polku.)
    salt-call
    sudo apt-get install salt-minion
    mkdir /etc/apt/keyrings
    cd /etc/apt/keyrings
    sudo curl -fsSL -o /etc/apt/keyrings/salt-archive-keyring-2023.gpg https://repo.saltproject.io/salt/py3/debian/12/amd64/SALT-PROJECT-GPG-PUBKEY-2023.gpg
    echo "deb [signed-by=/etc/apt/keyrings/salt-archive-keyring-2023.gpg arch=amd64] https://repo.saltproject.io/salt/py3/debian/12/amd64/latest bookworm main" | sudo tee /etc/apt/sources.list.d/salt.list
    sudo salt-call --local --file-root srv/salt/ state.apply
   
   
Aloitin ajamaan Salt-tiloja omasta varastostani ao. komennolla, mutta tämä ei tuottanut tulosta, herjasi command not foundia.

![image](https://github.com/katariinarytkonen/palvelintenhallinta/assets/164856665/56f72b62-43bd-4434-a727-818cf29b299b)

Tein hieman vianselvitystä, ja kävin aiemmasta kotitehtäväraportistani tarkistamassa erinäisiä salt-komentoja. Komento salt-call ei myöskään tuota tulosta, ja näin ilmeni että virtuaaliDebianillani ei ollutkaan Saltia asennettuna. Yritin asentaa tätä paketinhallinnan kautta, mutta ohjelma ei asentunut. Virheviestinä on Unable to locate package salt-minion

![image](https://github.com/katariinarytkonen/palvelintenhallinta/assets/164856665/8dfd4ea2-84d4-483e-95a6-76f7c53248bd)

Muistin että oppitunnilla tuli tämä sama ongelma ilmi, eikä juuri tässä kyseisessä Linux-versiossa (Debian 12 bookworm) ollut mahdollista asentaa Saltia tällä tavoin. Salt täytyi siis asentaa hieman perusteellisemmin kuin pelkällä sudo apt-get install salt-minion komennolla.

Löysin ohjeet ja tarvittavat komennot Saltin sivuilta(https://docs.saltproject.io/salt/install-guide/en/latest/topics/install-by-operating-system/debian.html#install-salt-on-debian-12-bookworm-amd64), ja valitsin oikean Linux-version, mitä olin asentamassa.
Sain sivulta tarvittavat komennot(listattu tehtävänannon jälkeen), joilla sain kuin sainkin Saltin asennettua Debianiin.

![image](https://github.com/katariinarytkonen/palvelintenhallinta/assets/164856665/17cb2fd2-9ad9-4a02-9cb4-3e5bc2bb675c)

Nyt kun testasin salt-callia uudestaan, oli kaikki toiminnassa, joten pääsin varsinaisen tehtävän pariin.

Annoin komennon sudo salt-call --local --file-root srv/salt/ state.apply, mutta virhe-ilmoitus tulee. 

![image](https://github.com/katariinarytkonen/palvelintenhallinta/assets/164856665/75794460-0d20-4962-837d-caa2cb9e61ac)

Kone ei löydä top-tiedostoa. Yritin löytää apuja google-haulla, ja luin tarkemmin top-tiedostosta. Tässä kohtaa ehkä täytyi käydä luomassa top.sls tiedosto srv/salt kansioon, jotta komento onnistuisi.

Käyn opettajan githubista hakemassa Salt-tiloja, ja yritän luoda top.sls-tiedostoa, mutta saan herjan että permission denied:

![image](https://github.com/katariinarytkonen/palvelintenhallinta/assets/164856665/a04ac7ea-5911-44f3-9a6c-4be5c95f2a9e)

Testaan muokata top.sls tiedostoa sudona, ja nyt onnistuu.

Käyn myös lisäämässä kansioon hello ja favourite-tilat, jotka saan opettajan sivuilta myöskin.

Tässä kohtaa aloin katsomaan polkua, ja aloin miettimään olisiko minun pitänyt vielä olla erikseen /srv/saltissa, kun nyt olen pelkässä /srv/, johon loin tiedostot..

![image](https://github.com/katariinarytkonen/palvelintenhallinta/assets/164856665/142631d8-c35d-4b5b-ab66-1e68f75fbbd8)

Muokkaan komentoa niin että poistan siitä polusta tuon saltin, jolloin näyttää toimivan tuon osalta, mutta tosin tästä tulee toinen virhe-ilmoitus:

![image](https://github.com/katariinarytkonen/palvelintenhallinta/assets/164856665/4d805671-7b49-4d1e-b032-8238875042c9)

Huomasin että syntaksissa oli virhe, toisessa tiedostossa taisi olla sana favourite monikossa ja toisessa oli yksikössä, joten eihän tämä tietenkään toimi. Korjasin tämän, ja ajoin komennon uudelleen, jolloin viimeinkin toimi.

![image](https://github.com/katariinarytkonen/palvelintenhallinta/assets/164856665/15d9161e-1f0f-4021-98c5-58d0a516bc17)

![image](https://github.com/katariinarytkonen/palvelintenhallinta/assets/164856665/726c3fad-6748-4e7e-8e7d-68035df59e34)

Tästä näkyy että asentui uusia paketteja, jotkut olin tosin testimielessä jo asentanut itse.





## Lähteet

Chacon and Straub 2014: Pro Git, 2ed: 1.3 Getting Started - What is Git? Luettavissa:
https://git-scm.com/book/en/v2/Getting-Started-What-is-Git%3F Luettu 14.4.2024

https://github.com/terokarvinen/suolax/blob/main/srv/salt/favourites/init.sls Luettu: 14.4.2024

https://github.com/terokarvinen/suolax/tree/main/srv/salt/hello Luettu 14.4.2024

https://github.com/terokarvinen/suolax/blob/main/srv/salt/top.sls Luettu 14.4.2024

https://docs.saltproject.io/en/latest/ref/states/top.html Luettu 14.4.2024

https://docs.saltproject.io/salt/install-guide/en/latest/topics/install-by-operating-system/debian.html#install-salt-on-debian-12-bookworm-amd64 Luettu 14.4.2024

Henry-Stocker 2023: The power of >, >>, &, &&, and || on Linux. Luettavissa:
https://www.networkworld.com/article/972419/the-power-of-and-on-linux.html Luettu 14.4.2024

Karvinen 2021: Salt Run Command Locally. Luettavissa:
https://terokarvinen.com/2021/salt-run-command-locally/ Luettu 14.4.2024

Karvinen 2024: Configuration Management 2024 Spring. Luettavissa:
https://terokarvinen.com/2024/configuration-management-2024-spring/ Luettu 13.4.2024







