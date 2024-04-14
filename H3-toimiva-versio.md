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
