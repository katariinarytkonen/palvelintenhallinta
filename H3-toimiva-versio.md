### a) Online. Tee uusi varasto GitHubiin (tai Gitlabiin tai mihin vain vastaavaan palveluun). Varaston nimessä ja lyhyessä kuvauksessa tulee olla sana "summer". Aiemmin tehty varasto ei kelpaa. 
 Luon uuden repositoryn GitHubiin graafisessa liittymässä, valitsen tälle nimen, lisään descriptionin ja readme.filen ja lisenssiksi GNU General Public License 3.
 
 ![image](https://github.com/katariinarytkonen/palvelintenhallinta/assets/164856665/93ae00fa-1b92-4fd2-b49e-35c60120f979)

 ### b) Dolly. Kloonaa edellisessä kohdassa tehty uusi varasto itsellesi, tee muutoksia omalla koneella, puske ne palvelimelle, ja näytä, että ne ilmestyvät weppiliittymään.
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

### c) Doh! Tee tyhmä muutos gittiin, älä tee commit:tia. Tuhoa huonot muutokset ‘git reset --hard’. Huomaa, että tässä toiminnossa ei ole peruutusnappia.

    git add
    git reset --hard
    
Oletan tyhmän muutoksen tarkoittavan esim jotain tiedostoa, mitä ei ollut tarkoittanut laittaa julkiseksi, joten luon uuden tiedoston testirepositoryyn, lisään tämän gitiin git add-komennolla,  jonka poistankin ennen kuin ajan näitä GitHubiin. 

![image](https://github.com/katariinarytkonen/palvelintenhallinta/assets/164856665/28ab002f-2b33-4b49-930b-2e9a3664ef8f)

![image](https://github.com/katariinarytkonen/palvelintenhallinta/assets/164856665/05f5e549-0e39-488e-b3aa-4651a171d625)

Varmistan vielä että poistuihan väärä tiedostoni; poistui.
![image](https://github.com/katariinarytkonen/palvelintenhallinta/assets/164856665/d6e97f0d-faf9-48c3-bf05-e70f798d6aed)

### Tukki. Tarkastele ja selitä varastosi lokia. Tarkista, että nimesi ja sähköpostiosoitteesi näkyy haluamallasi tavalla ja korjaa tarvittaessa.

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

## Lähteet









