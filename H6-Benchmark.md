### x) Lue ja tiivistä. (Tässä x-alakohdassa ei tarvitse tehdä testejä tietokoneella, vain lukeminen tai kuunteleminen ja tiivistelmä riittää. Tiivistämiseen riittää muutama ranskalainen viiva.)Windows Package Manager: Introduction, Install libraries, Populate the local Git repository, Update minion database, Install software package, Usage osa. Eli sivun alusta kappaleen "Remove a package" loppuun, poislukien "Configuration". (Kannatta soveltaa asennuksesta idempotentti versio, ulkomuistista: 'sudo salt-call --local -l info state.apply pkg.installed curl').

- Windows Package Managerilla saa Salt-tilalla Linuxista tutun paketinhallinnan myös Windowsille. Tällä tavoin saa helposti ja nopeasti asennettua, päivitettyä ja poistettua ohjelmia.
- Jotta tämä toimii, täytyy Git olla asennettuna koneella, sillä tämä hakee windows-paketit git-repositorysta
- $ salt-call --local winrepo.update_git_repos
- $ salt-call --local pkg.refresh_db
- $ salt-call --local pkg.install "firefox_x64"



### a) Paketti Windowsia. Asenna Windowsiin tai Macciin ohjelmia Saltin pkg.installed -funktiolla. (Jos teit tarvittavat asennukset jo tunnilla, voit kirjoittaa ympäristön asennuksen ulkomuistista, ja asentaa nyt vain jonkin uuden paketin. Muistinvaraisesti kirjoitetut muistiinpanot ovat paljon epämääräisempiä kuin työn aikana kirjoitetut, joten merkitse selvästi, mitkä osat on kirjoitettu ulkomuistista ja mitkä työskennellessä.)

Olin tehnyt tarvittavat asennukset jo tunnilla, mutta kirjasin ylläolevaan lue ja tiivistä tehtävään olennaiset komennot, joilla ympäristö asennetaan itsenäiselle koneelle, jolla ei ole masteria.

![image](https://github.com/katariinarytkonen/palvelintenhallinta/assets/164856665/fd1fbe31-a448-47c0-ba9d-9449dfb13909).

Täältä näkee mitä ohjelmia ylipäätään oli mahdollista asentaa Saltiin tuolla pkg.installed -funktiolla:

https://github.com/saltstack/salt-winrepo-ng 

Testasin paketinhallinnan toimivuutta asentamalla uuden ohjelman, inkscapen. Inkscape on ohjelmisto, jolla voi tuottaa vektorigrafiikkaa. Asennus seuraavalla komennolla:

     salt-call --local pkg.install "inkscape"

Testasin että ohjelma aukeni myös ihan käyttöjärjestelmässä, ja lopuksi poistin tämän itselleni turhana.

![image](https://github.com/katariinarytkonen/palvelintenhallinta/assets/164856665/779833f9-9d62-45df-b711-6917b1faa69b)

![image](https://github.com/katariinarytkonen/palvelintenhallinta/assets/164856665/060628df-403a-406e-a2c8-548564e4eef9)

     salt-call --local pkg.removed "inkscape"



### b) Benchmark. Etsi 3-7 keskitetyn hallinnan projektia, esimerkiksi tämän kurssin "Oma moduli" lopputyötä. Työn tulee olla modernia keskitettyä hallintaa (idempotentti, infra koodina, yksi totuus). Esimerkiksi pelkkä shell script ei kelpaa. Työ saa käyttää mitä vain työkalua, esim Salt, Puppet, Ansible, Chef, Conftero, CFEngine. Tässä alakohdassa ei tehdä mitään testejä, vaan arvioidaan töitä vain niiden kotisivujen perusteella. Etsimiseen voit käyttää hakukoneita.

1. https://tgratschew.news.blog/2020/05/18/palvelinten-hallinta-ict4tn022-3005-harjoitus-7/

Projektin tarkoitus oli tehdä oma livemoduuli, joka konfiguroi palomuurin, ssh-yhteyden ja apachen minioneille. Lisenssistä ei ole sivulla missään mainintaa. Tekijä Tommy Gratschew, vuosi 2020. 
Riippuvuuksina on listattu Oracle VirtualBox 6.1.6 ja Xubuntu 18.04.03 LTS (+ VirtualBox guest additions).
Tässä kiinnostavaa oli se, että moduulilla asennettiin ja konffattiin tärkeitä ja olennaisia perusjuttuja koneelle.  

2. https://joonakarvonen.design.blog/palvelinten-hallinta-joona-karvonen-harjoitus-7/

Projektin tarkoitus oli asentaa Saltilla Minecraft-palvelin orjakoneelle, joka toimii lähiverkossa. Tämä oli kirjoittajan mukaan hyödyllinen esim Lan-tapahtumissa, jotta useampi pelaaja voi liittyä pelaamaan yhdessä peliä samalle palvelimelle.
Lisenssistä ei ole mainintaa. Tekijä on Joona Karvonen, vuosi 2020.
Riippuvuus: erillinen pöytäkone, käyttöjärjestelmänä Ubuntu 18.04.
Tässä kiinnostavaa ja hienoa oli se, että tekijä oli tehnyt moduulin aidosti omaan tarpeeseensa, ja luonut samalla omien sanojensa mukaisesti jotain täysin jotain uutta, sillä hän veikkasi että kukaan ei ole aiemmin asentanut Saltilla minecraft-palvelinta. 

3. https://github.com/Eetu95/salt
   
Projektin tarkoituksena oli tehdä SaltStack-miniprojekti, joka asensi VirtualBoxin Linuxille ja että sitä pystyi käyttää selaimessa(phpVirtualBox).
Lisenssinä oli GNU GPL versio 2. tai uudempi. Linsenssin tiedot lukevat projektin lopussa, sekä GITissä ylempänä: ![image](https://github.com/katariinarytkonen/palvelintenhallinta/assets/164856665/1e7ead4b-9650-45d9-90b8-e6705855f616)

Linsenssi antaa kenelle tahansa oikeuden käyttää, kopioida, muuttaa ja jakaa edelleen ohjelmia ja niiden lähdekoodia. 
Projektin tekijä on ollut Eetu95, koko nimeä ei ole näkyvillä hänen Githubissa, joten voi vain arvailla. Projekti on vuodelta 2018.
Riippuvuudet: Virtualbox VM-ympäristö, siihen asennettu Linux Xubuntu 18.04 Desktop AMD64 sekä Linux Ubuntu Server 16.04.5 LTS 64-bit virtuaalikoneet.
Tässä kiinnostavaa oli mahdollisuus käyttää ohjelmaa selaimessa.

4. https://lassekiljunen.wordpress.com/harjoitus-7/

Projektin tarkoituksena oli asentaa ja konfiguroida Nsnake-peli kahdelle eri koneelle ja toimia viihdykkeenä esim kahvitauolla.
Linsenssistä ei ole mainintaa. Tekijä on Lasse Kiljunen, vuosi 2020.
Riippuvuudet: Virtualbox ja joku linuxilla pyörivä virtuaalikone.
Tässä kiinnosti tuo aihe, kukapa nyt ei kaipaa hauskoja pelejä koneelleen, ja matopeli on klassikko.

5. https://konstavaarala.wordpress.com/2017/05/11/oma-puppet-moduuli/

Projektin tarkoituksena oli tehdä Puppet-moduuli, joka asentaa selaimen(Chrome) ja vaihtaa työpöydän taustakuvan.

Tässä kiinnostavaa oli se, että tässä oli käytetty Saltin sijasta Puppetia, joka on itselleni täysin vieras ohjelma. 

6. https://ottotoivanen.wordpress.com/2020/05/21/palvelinten-hallinta-h7/

Projektin tarkoituksena oli tehdä oma master-kone, jonka avulla voisi hallita minioneita, joille tulisi olla asennettuna Steam ja Discord.
Lisenssistä ei mainintaa. Tekijä on Otto Toivanen, vuosi 2020.
Riippuvuudet:


### c) Testbench. Aja toisen tekemä tila.

Päädyin valitsemaan testiini Lasse Kiljusen Snake-pelin asennuksen, sillä peli vaikutti hauskalta.
Toimin ohjeiden mukaan ja loin uuden nsnake-directoryn, johon tein init.sls -tiedoston ao. tekstillä: 

![image](https://github.com/katariinarytkonen/palvelintenhallinta/assets/164856665/b78203a9-4c7f-4640-b184-e467e4db076f)
![image](https://github.com/katariinarytkonen/palvelintenhallinta/assets/164856665/97791d81-7443-465b-95f2-f207932733f1)

Yritin ajaa sitä sudo salt '*' state.apply nsnake -komennolla, mutta ei toiminut. Salt kuitenkin näytti toimivan, joten siirryin testaamaan jotain muuta moduulia.






### d) Viisi ideaa. Listaa viisi ideaa omalle modulille, kurssin lopputehtävälle. Modulilla tulee olla tarkoistus. Sen ei tarvitse silti ratkaista mitään oikeaa liiketoiminnan ongelmaa, vaan tarkoitus voi olla keksitty. Kunkin idean kuvaukseen riittää yksi virke. 

1. Moduuli, joka asentaa jotkut tietyt peruskonfiguroinnit uuteen koneeseen.
2. Moduuli, joka asentaa kahdelle virtuaalikoneelle tietyt ohjelmat(esim micro ja apache
3. Moduuli, jolla asentaa uuden 
4.
5.


### Lähteet:

https://docs.saltproject.io/en/latest/topics/windows/windows-package-manager.html Luettu 5.5.2024

https://fi.wikipedia.org/wiki/Inkscape Luettu 5.5.2024 

Gratschew, 2020. Palvelinten hallinta ICT4TN022-3005 Harjoitus 7. Luettavissa: 

https://tgratschew.news.blog/2020/05/18/palvelinten-hallinta-ict4tn022-3005-harjoitus-7/ Luettu 5.5.2024

Karvonen 2020. Palvelinten Hallinta Joona Karvonen Harjoitus 7. Luettavissa:

https://joonakarvonen.design.blog/palvelinten-hallinta-joona-karvonen-harjoitus-7/ Luettu 5.5.2024.

https://github.com/Eetu95/salt Luettu 5.5.2024

https://www.gnu.org/licenses/quick-guide-gplv3.html Luettu 5.5.2024

Vaarala 2017. Oma Puppet Moduuli. Luettavissa:

https://konstavaarala.wordpress.com/2017/05/11/oma-puppet-moduuli/ Luettu 5.5.2024

Toivanen, 2020. Palvelinten hallinta – h7. Luettavissa:

https://ottotoivanen.wordpress.com/2020/05/21/palvelinten-hallinta-h7/ Luettu 5.5.2024
