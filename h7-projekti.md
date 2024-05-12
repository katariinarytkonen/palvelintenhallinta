## Salt-projektini

Seuraavana vuorossa on kurssin loppuharjoituksen tekeminen-

Projektin tekijänä on Katariina Rytkönen ja projektini lisenssinä on GNU General Public License, versio 3.

Moduulin ideana on saada asennettua esim työpaikan uusille tietokoneille työpaikalla käytettävät oletusohjelmat kerralla keskitetyllä hallinnalla Saltin avulla.

Tässä kuvitteellisessa tilanteessa työntekijä tarvitsee koneelleen työnantajan ennalta hyväksymät ohjelmat joita tarvitsee työssään; Mozilla Firefoxin, Gitin, Micron, Libreofficen ja Thunderbirdin.
Lisäksi koneelle tulee konfiguroida palomuuri.
pwd
### Virtuaaliympäristön pystyttäminen 

Aloitan moduulin tekemisen luomalla virtuaaliympäristön, eli kaksi konetta Vagrantilla Teron ohjeilla. (https://terokarvinen.com/2021/two-machine-virtual-network-with-debian-11-bullseye-and-vagrant/?fromSearch=two%20machine%20virtual)

    mkdir twohost
    cd twohost
    nano Vagrantfile
    vagrant up
    vagrant ssh t001

![image](https://github.com/katariinarytkonen/palvelintenhallinta/assets/164856665/124a55ad-9592-4e33-bb87-fdce9f8e189a)

### Herra-orja-arkkitehtuurin määrittely ja hei maailma

Seuraavaksi asensin koneille herra-orja-arkkitehtuurin niin, että t001 on herra, ja t002 orja oman kurssisaporttini ohjeella: https://github.com/katariinarytkonen/palvelintenhallinta/blob/main/H2%20Soitto%20Kotiin.md. Koska tämä on tuolla jo kuvattu kuva kuvalta, en lähde täyttämään nyt omaa projektiani näiden ohjeilla sen tarkemmin.

Kun ympäristö oli toiminnassa ja pystyssä, pääsin luomaan omaa moduuliani. Aloitin moduulin luomisen hei maailmalla, jotta näen kaiken toimivan.

Hei maailma-moduuliani varten loin salt-kansioon hello-kansion, johon loin sudoeditillä init.sls tiedoston, jossa seuraavanlainen sisältö:

![image](https://github.com/katariinarytkonen/palvelintenhallinta/assets/164856665/708a7fe5-d79d-4ee1-9ab2-38189d9fd97f)

euraavaksi ajoin tilan `sudo salt '*' state.apply hello` -komennolla, ja tarkistin että kaikki toimii:

![image](https://github.com/katariinarytkonen/palvelintenhallinta/assets/164856665/16ab149f-d2b0-4e87-ad3d-d4a89fd90988)

Nyt pääsinkin seuraavaan vaiheeseen.

### Oma moduuli

Loin /srv/saltiin uuden directoryn, omamoduulin. Luon tänne uuden tiedoston ao. sisällöllä `micro init.sls`

![image](https://github.com/katariinarytkonen/palvelintenhallinta/assets/164856665/633a6f48-1c3f-4636-94d0-8114d3818c40)

Lisäksi luon /srv/saltiin top-filen, top.sls johon määrään omamoduulin ajettavaksi ohjelmaksi.

![image](https://github.com/katariinarytkonen/palvelintenhallinta/assets/164856665/5d43aa14-3608-4bc8-bbec-172a9945ec1e)

Testaan tämän toimintaa ensin paikallisesti `sudo salt-call --local state.apply omamoduuli`. 

Tämä toimii, tosin ongelmia tuotti ladattavien pakettien nimet, sillä kaikki ohjelmistot eivät asentuneetkaan ihan niin tarkasti kun olisin toivonut. Testasin libreofficen asennusta, ja se  kesti pienen ikuisuuden, sillä se asensikin monia muitakin ohjelmia, jotka ilmeisesti ovat tämän toiminnalle olennaisia.



local:
----------
          ID: omamoduuli
    Function: pkg.installed
      Result: True
     Comment: The following packages were installed/updated: libreoffice
              The following packages were already installed: git, firefox-esr, micro
     Started: 16:52:44.875373
    Duration: 273044.93 ms
     Changes:   
              ----------
              libapache-poi-java:
                  ----------
                  new:
                      4.0.1-1
                  old:
              libargs4j-java:
                  ----------
                  new:
                      2.33-1.1
                  old:
              libatinject-jsr330-api-java:
                  ----------
                  new:
                      1.0+ds1-5
                  old:
              libbase-java:
                  ----------
                  new:
                      1.1.6-2.1
                  old:
              libbcmail-java:
                  ----------
                  new:
                      1.68-2
                  old:
              libbcpkix-java:
                  ----------
                  new:
                      1.68-2
                  old:
              libbcprov-java:
                  ----------
                  new:
                      1.68-2
                  old:
              libbsh-java:
                  ----------
                  new:
                      2.0b4-20
                  old:
              libcdi-api-java:
                  ----------
                  new:
                      1.2-3
                  old:
              libcdr-0.1-1:
                  ----------
                  new:
                      0.1.6-2
                  old:
              libcodemodel-java:
                  ----------
                  new:
                      2.6+jaxb2.3.0.1-10
                  old:
              libcolamd2:
                  ----------
                  new:
                      1:5.8.1+dfsg-2
                  old:
              libcommons-cli-java:
                  ----------
                  new:
                      1.4-2
                  old:
              libcommons-collections3-java:
                  ----------
                  new:
                      3.2.2-2
                  old:
              libcommons-io-java:
                  ----------
                  new:
                      2.8.0-1
                  old:
              libcommons-lang3-java:
                  ----------
                  new:
                      3.11-1
                  old:
              libcommons-math3-java:
                  ----------
                  new:
                      3.6.1-3
                  old:
              libcurvesapi-java:
                  ----------
                  new:
                      1.06-1
                  old:
              libdom4j-java:
                  ----------
                  new:
                      2.1.3-1
                  old:
              libdtd-parser-java:
                  ----------
                  new:
                      1.2-1
                  old:
              libe-book-0.1-1:
                  ----------
                  new:
                      0.1.3-2
                  old:
              libehcache-java:
                  ----------
                  new:
                      2.6.11-5
                  old:
              libepubgen-0.1-1:
                  ----------
                  new:
                      0.1.1-1
                  old:
              libetonyek-0.1-1:
                  ----------
                  new:
                      0.1.9-4
                  old:
              libfastinfoset-java:
                  ----------
                  new:
                      1.2.12-3
                  old:
              libflute-java:
                  ----------
                  new:
                      1:1.1.6-4
                  old:
              libfonts-java:
                  ----------
                  new:
                      1.1.6.dfsg-3.1
                  old:
              libformula-java:
                  ----------
                  new:
                      1.1.7.dfsg-2.1
                  old:
              libfreehand-0.1-1:
                  ----------
                  new:
                      0.1.2-3
                  old:
              libgeronimo-annotation-1.3-spec-java:
                  ----------
                  new:
                      1.3-1
                  old:
              libgeronimo-interceptor-3.0-spec-java:
                  ----------
                  new:
                      1.0.1-4
                  old:
              libguava-java:
                  ----------
                  new:
                      29.0-6
                  old:
              libguice-java:
                  ----------
                  new:
                      4.2.3-2
                  old:
              libhawtjni-runtime-java:
                  ----------
                  new:
                      1.17-1
                  old:
              libhttpclient-java:
                  ----------
                  new:
                      4.5.13-2
                  old:
              libhttpcore-java:
                  ----------
                  new:
                      4.4.14-1
                  old:
              libicu4j-java:
                  ----------
                  new:
                      68.2-2
                  old:
              libintellij-annotations-java:
                  ----------
                  new:
                      20.1.0-1
                  old:
              libistack-commons-java:
                  ----------
                  new:
                      3.0.6-5
                  old:
              libitext-java:
                  ----------
                  new:
                      2.1.7-12
                  old:
              libjansi-java:
                  ----------
                  new:
                      1.18-1
                  old:
              libjansi-native-java:
                  ----------
                  new:
                      1.8-1
                  old:
              libjaxb-api-java:
                  ----------
                  new:
                      2.3.1-1
                  old:
              libjaxb-java:
                  ----------
                  new:
                      2.3.0.1-10
                  old:
              libjaxen-java:
                  ----------
                  new:
                      1.1.6-4
                  old:
              libjcommon-java:
                  ----------
                  new:
                      1.0.23-2
                  old:
              libjdom1-java:
                  ----------
                  new:
                      1.1.3-2.1
                  old:
              libjetbrains-annotations-java:
                  ----------
                  new:
                      20.1.0-1
                  old:
              libjsoup-java:
                  ----------
                  new:
                      1.10.2-2
                  old:
              libjsr305-java:
                  ----------
                  new:
                      0.1~+svn49-11
                  old:
              libjuh-java:
                  ----------
                  new:
                      1:7.0.4-4+deb11u8
                  old:
              libjurt-java:
                  ----------
                  new:
                      1:7.0.4-4+deb11u8
                  old:
              liblayout-java:
                  ----------
                  new:
                      0.2.10-3.1
                  old:
              liblibreoffice-java:
                  ----------
                  new:
                      1:7.0.4-4+deb11u8
                  old:
              libloader-java:
                  ----------
                  new:
                      1.1.6.dfsg-4.1
                  old:
              liblog4j1.2-java:
                  ----------
                  new:
                      1.2.17-10+deb11u1
                  old:
              libmail-java:
                  ----------
                  new:
                      1.6.5-1
                  old:
              libmariadb3:
                  ----------
                  new:
                      1:10.5.23-0+deb11u1
                  old:
              libmaven-file-management-java:
                  ----------
                  new:
                      3.0.0-1
                  old:
              libmaven-parent-java:
                  ----------
                  new:
                      31-2
                  old:
              libmaven-resolver-java:
                  ----------
                  new:
                      1.4.2-3
                  old:
              libmaven-shared-io-java:
                  ----------
                  new:
                      3.0.0-3
                  old:
              libmaven-shared-utils-java:
                  ----------
                  new:
                      3.3.0-1+deb11u1
                  old:
              libmaven3-core-java:
                  ----------
                  new:
                      3.6.3-5
                  old:
              libmspub-0.1-1:
                  ----------
                  new:
                      0.1.4-3+b1
                  old:
              libmwaw-0.3-3:
                  ----------
                  new:
                      0.3.17-1
                  old:
              libodfgen-0.1-1:
                  ----------
                  new:
                      0.1.8-2
                  old:
              libpagemaker-0.0-0:
                  ----------
                  new:
                      0.0.4-1
                  old:
              libpentaho-reporting-flow-engine-java:
                  ----------
                  new:
                      0.9.4-5.1
                  old:
              libpixie-java:
                  ----------
                  new:
                      1:1.1.6-3.1
                  old:
              libplexus-archiver-java:
                  ----------
                  new:
                      3.6.0-2
                  old:
              libplexus-cipher-java:
                  ----------
                  new:
                      1.8-2
                  old:
              libplexus-classworlds-java:
                  ----------
                  new:
                      2.6.0-1
                  old:
              libplexus-component-annotations-java:
                  ----------
                  new:
                      2.1.0-1
                  old:
              libplexus-interpolation-java:
                  ----------
                  new:
                      1.26-1
                  old:
              libplexus-io-java:
                  ----------
                  new:
                      3.2.0-1.1
                  old:
              libplexus-sec-dispatcher-java:
                  ----------
                  new:
                      1.4-4
                  old:
              libplexus-utils2-java:
                  ----------
                  new:
                      3.3.0-1
                  old:
              libpq5:
                  ----------
                  new:
                      13.14-0+deb11u1
                  old:
              libpython3.9:
                  ----------
                  new:
                      3.9.2-1
                  old:
              libqxp-0.0-0:
                  ----------
                  new:
                      0.0.2-1+b1
                  old:
              librelaxng-datatype-java:
                  ----------
                  new:
                      1.0+ds1-3.1
                  old:
              libreoffice:
                  ----------
                  new:
                      1:7.0.4-4+deb11u8
                  old:
              libreoffice-calc:
                  ----------
                  new:
                      1:7.0.4-4+deb11u8
                  old:
              libreoffice-draw:
                  ----------
                  new:
                      1:7.0.4-4+deb11u8
                  old:
              libreoffice-impress:
                  ----------
                  new:
                      1:7.0.4-4+deb11u8
                  old:
              libreoffice-java-common:
                  ----------
                  new:
                      1:7.0.4-4+deb11u8
                  old:
              libreoffice-math:
                  ----------
                  new:
                      1:7.0.4-4+deb11u8
                  old:
              libreoffice-nlpsolver:
                  ----------
                  new:
                      0.9+LibO7.0.4-4+deb11u8
                  old:
              libreoffice-report-builder:
                  ----------
                  new:
                      1:7.0.4-4+deb11u8
                  old:
              libreoffice-report-builder-bin:
                  ----------
                  new:
                      1:7.0.4-4+deb11u8
                  old:
              libreoffice-script-provider-bsh:
                  ----------
                  new:
                      1:7.0.4-4+deb11u8
                  old:
              libreoffice-script-provider-js:
                  ----------
                  new:
                      1:7.0.4-4+deb11u8
                  old:
              libreoffice-script-provider-python:
                  ----------
                  new:
                      1:7.0.4-4+deb11u8
                  old:
              libreoffice-sdbc-mysql:
                  ----------
                  new:
                      1:7.0.4-4+deb11u8
                  old:
              libreoffice-sdbc-postgresql:
                  ----------
                  new:
                      1:7.0.4-4+deb11u8
                  old:
              libreoffice-wiki-publisher:
                  ----------
                  new:
                      1.2.0+LibO7.0.4-4+deb11u8
                  old:
              libreoffice-writer:
                  ----------
                  new:
                      1:7.0.4-4+deb11u8
                  old:
              librepository-java:
                  ----------
                  new:
                      1.1.6-4
                  old:
              libridl-java:
                  ----------
                  new:
                      1:7.0.4-4+deb11u8
                  old:
              librngom-java:
                  ----------
                  new:
                      2.3.0.1-10
                  old:
              libsac-java:
                  ----------
                  new:
                      1.3+dfsg-5.1
                  old:
              libsaxonhe-java:
                  ----------
                  new:
                      9.9.1.5+dfsg-1
                  old:
              libserializer-java:
                  ----------
                  new:
                      1.1.6-6
                  old:
              libsisu-inject-java:
                  ----------
                  new:
                      0.3.4-2
                  old:
              libsisu-plexus-java:
                  ----------
                  new:
                      0.3.4-3
                  old:
              libslf4j-java:
                  ----------
                  new:
                      1.7.30-1
                  old:
              libsnappy-java:
                  ----------
                  new:
                      1.1.8.3-1
                  old:
              libsnappy-jni:
                  ----------
                  new:
                      1.1.8.3-1
                  old:
              libstaroffice-0.0-0:
                  ----------
                  new:
                      0.0.7-1
                  old:
              libstax-ex-java:
                  ----------
                  new:
                      1.7.8-3
                  old:
              libstreambuffer-java:
                  ----------
                  new:
                      1.5.4-1.1
                  old:
              libsuitesparseconfig5:
                  ----------
                  new:
                      1:5.8.1+dfsg-2
                  old:
              libtxw2-java:
                  ----------
                  new:
                      2.3.0.1-10
                  old:
              libunoil-java:
                  ----------
                  new:
                      1:7.0.4-4+deb11u8
                  old:
              libunoloader-java:
                  ----------
                  new:
                      1:7.0.4-4+deb11u8
                  old:
              libvisio-0.1-1:
                  ----------
                  new:
                      0.1.7-1+b1
                  old:
              libwagon-http-java:
                  ----------
                  new:
                      3.3.4-1
                  old:
              libwagon-provider-api-java:
                  ----------
                  new:
                      3.3.4-1
                  old:
              libwpd-0.10-10:
                  ----------
                  new:
                      0.10.3-1
                  old:
              libwpg-0.3-3:
                  ----------
                  new:
                      0.3.3-1
                  old:
              libwps-0.4-4:
                  ----------
                  new:
                      0.4.12-1
                  old:
              libxerces2-java:
                  ----------
                  new:
                      2.12.1-1
                  old:
              libxml-commons-external-java:
                  ----------
                  new:
                      1.4.01-5
                  old:
              libxml-commons-resolver1.1-java:
                  ----------
                  new:
                      1.2-11
                  old:
              libxml-java:
                  ----------
                  new:
                      1.1.6.dfsg-3.1
                  old:
              libxmlbeans-java:
                  ----------
                  new:
                      3.0.2-1
                  old:
              libxom-java:
                  ----------
                  new:
                      1.2.10-1.1
                  old:
              libxsom-java:
                  ----------
                  new:
                      2.3.0.1-10
                  old:
              libxz-java:
                  ----------
                  new:
                      1.8-2
                  old:
              libzmf-0.0-0:
                  ----------
                  new:
                      0.0.2-1+b3
                  old:
              lp-solve:
                  ----------
                  new:
                      5.5.2.5-2
                  old:
              mariadb-common:
                  ----------
                  new:
                      1:10.5.23-0+deb11u1
                  old:
              mysql-common:
                  ----------
                  new:
                      5.8+1.0.7
                  old:
              python3-uno:
                  ----------
                  new:
                      1:7.0.4-4+deb11u8
                  old:

Summary for local
------------
Succeeded: 1 (changed=1)
Failed:    0
------------



Tämä on vähän huono tietysti sen puolesta, että kun asennan nämä orjilleni, kestää tässä taas ikuisuus. Harkitsin että korvaisin thunderbirdin ja libreofficen joillain toisilla ohjelmilla, jotka eivät asentaisi samalla miljoonaa muuta ohjelmaa, sillä nämä ovat vähän huono tälleen demomielessä..
Jätän tämän pohdintaan ja siirryn seuraavaan vaiheeseen.

### Palomuurin asentaminen

Luon uuden directoryn, `/srv/salt/palomuuri`.

Valitsen palumuuriksi ufwn. 

Tänne teen uuden tiedoston init.sls, johon teen palomuurin määrityksiä.

![image](https://github.com/katariinarytkonen/palvelintenhallinta/assets/164856665/1a72df5e-78ed-4592-8512-c3daa7570b50)

Selvästi koodissani oli jotain vikaa, sillä tämä jäi pyörimään taas kymmeniksi minuuteiksi.

![image](https://github.com/katariinarytkonen/palvelintenhallinta/assets/164856665/cd0ee70e-b44b-4878-8957-3f561f57b79d)

Jossain kohtaa painoin enteriä, ja asennus keskeytyi. Mutta näyttäisi kuitenkin toimivan jotenkuten. 

![image](https://github.com/katariinarytkonen/palvelintenhallinta/assets/164856665/b26a1f51-4587-45ac-9e8e-c6014d1f057b)

Lisään top.sls tiedostooni myös palomuuri-moduulini, ja testaan ajaa tämän ensin paikallisesti, sitten orjilla.

![image](https://github.com/katariinarytkonen/palvelintenhallinta/assets/164856665/4a0c8487-b53d-4fb1-9fcf-e311c360068a)

sudo salt-call --local -l info state.apply
sudo salt '*' -l info state.apply

![image](https://github.com/katariinarytkonen/palvelintenhallinta/assets/164856665/7fbaffdb-da2a-4737-b12e-5f5642bdfca0)

![image](https://github.com/katariinarytkonen/palvelintenhallinta/assets/164856665/e88f515d-f5bc-4c3c-ad18-44921a444b4c)

Tilan ajaminen orjalla ei nyt onnistukaan. Käyn ottamassa moduulieni tiedot talteen, ja pystytän uuden virtuaaliympäristön.












### Lähteet

Karvinen 2024: Configuration management 2024 spring. Luettavissa:

https://terokarvinen.com/2024/configuration-management-2024-spring/. Luettu 12.5.2024

Karvinen 2021: Two Machine Virtual Network With Debian 11 Bullseye and Vagrant. Luettavissa:

https://terokarvinen.com/2021/two-machine-virtual-network-with-debian-11-bullseye-and-vagrant/ Luettu 12.5.2024

Karvinen 2016: Instant Firewall Sudo Ufw Enable. Luettavissa:

https://terokarvinen.com/2016/instant-firewall-sudo-ufw-enable/?fromSearch=firewall Luettu 12.5.2024

https://github.com/katariinarytkonen/palvelintenhallinta/blob/main/H2%20Soitto%20Kotiin.md Luettu 12.5.2024

https://wiki.debian.org/Firefox Luettu 12.5.2024
