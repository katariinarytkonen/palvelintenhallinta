### x) Lue ja tiivistä. (Tässä x-alakohdassa ei tarvitse tehdä testejä tietokoneella, vain lukeminen tai kuunteleminen ja tiivistelmä riittää. Tiivistämiseen riittää muutama ranskalainen viiva.)Windows Package Manager: Introduction, Install libraries, Populate the local Git repository, Update minion database, Install software package, Usage osa. Eli sivun alusta kappaleen "Remove a package" loppuun, poislukien "Configuration". (Kannatta soveltaa asennuksesta idempotentti versio, ulkomuistista: 'sudo salt-call --local -l info state.apply pkg.installed curl').

- Windows Package Managerilla saa Salt-tilalla Linuxista tutun paketinhallinnan myös Windowsille. Tällä tavoin saa helposti ja nopeasti asennettua, päivitettyä ja poistettua ohjelmia.
- Jotta tämä toimii, täytyy Git olla asennettuna koneella, sillä tämä hakee windows-paketit git-repositorysta
- $ salt-call --local winrepo.update_git_repos
- $ salt-call --local pkg.refresh_db
- $ salt-call --local pkg.install "firefox_x64



### a) Paketti Windowsia. Asenna Windowsiin tai Macciin ohjelmia Saltin pkg.installed -funktiolla. (Jos teit tarvittavat asennukset jo tunnilla, voit kirjoittaa ympäristön asennuksen ulkomuistista, ja asentaa nyt vain jonkin uuden paketin. Muistinvaraisesti kirjoitetut muistiinpanot ovat paljon epämääräisempiä kuin työn aikana kirjoitetut, joten merkitse selvästi, mitkä osat on kirjoitettu ulkomuistista ja mitkä työskennellessä.)

### b) Benchmark. Etsi 3-7 keskitetyn hallinnan projektia, esimerkiksi tämän kurssin "Oma moduli" lopputyötä. Työn tulee olla modernia keskitettyä hallintaa (idempotentti, infra koodina, yksi totuus). Esimerkiksi pelkkä shell script ei kelpaa. Työ saa käyttää mitä vain työkalua, esim Salt, Puppet, Ansible, Chef, Conftero, CFEngine. Tässä alakohdassa ei tehdä mitään testejä, vaan arvioidaan töitä vain niiden kotisivujen perusteella. Etsimiseen voit käyttää hakukoneita.

### c) Testbench. Aja toisen tekemä tila.

### d) Viisi ideaa. Listaa viisi ideaa omalle modulille, kurssin lopputehtävälle. Modulilla tulee olla tarkoistus. Sen ei tarvitse silti ratkaista mitään oikeaa liiketoiminnan ongelmaa, vaan tarkoitus voi olla keksitty. Kunkin idean kuvaukseen riittää yksi virke. 

### Lähteet:

https://docs.saltproject.io/en/latest/topics/windows/windows-package-manager.html Luettu 5.5.2024
