## Salt-projektini

Seuraavana vuorossa on kurssin loppuharjoitus, miniprojekti Saltilla toteutettuna.

Projektin tekijänä on Katariina Rytkönen ja projektini lisenssinä on GNU General Public License, versio 3.

Moduulin ideana on saada asennettua esim työpaikan uusille tietokoneille firmassa käytettävät oletusohjelmat kerralla keskitetyllä hallinnalla Saltin avulla, ja lisäksi tässä konfiguroidaan koneille palomuuri.

Tässä kuvitteellisessa tilanteessa työntekijä tarvitsee koneelleen työnantajan ennalta hyväksymät ohjelmat joita tarvitsee työssään; Mozilla Firefoxin, Gitin, Micron, Libreofficen ja Thunderbirdin. Thunderbird emailiin, libreoffice tiedostojenkäsittelyyn, micro tekstieditoriksi, Git versionhallintaan ja firefox selaimeksi.

Lähdekoodi tarkemmin luettavissa toisessa projekti-repossa:

https://github.com/katariinarytkonen/projekti/tree/main

### Virtuaaliympäristön pystyttäminen 

Aloitan moduulin tekemisen luomalla virtuaaliympäristön, eli kaksi konetta Vagrantilla Teron ohjeilla. (https://terokarvinen.com/2021/two-machine-virtual-network-with-debian-11-bullseye-and-vagrant/?fromSearch=two%20machine%20virtual)

    mkdir twohost
    cd twohost
    nano Vagrantfile
    vagrant up
    vagrant ssh t001

![image](https://github.com/katariinarytkonen/palvelintenhallinta/assets/164856665/124a55ad-9592-4e33-bb87-fdce9f8e189a)

### Herra-orja-arkkitehtuurin määrittely ja hei maailma

Seuraavaksi asensin koneille herra-orja-arkkitehtuurin niin, että t001 on herra, ja t002 orja oman kurssisaporttini ohjeella: https://github.com/katariinarytkonen/palvelintenhallinta/blob/main/H2%20Soitto%20Kotiin.md. 

 Asensin t001 herran komennolla `sudo apt-get -y install salt-master`. Selvitin ip-osoitteen komennolla `hostname -I`, sillä tämä täytyy lisätä minionille jotta tunnistaa master-koneen. Poistuin yhteydestä `exit`-komennolla asentaakseni toisesta koneesta orjan. Komennolla `sudoedit /etc/salt/minion` menin lisäämään tiedostoon herran ip-osoitteen, jotta orja tunnistaa herran ja kaikki toimii kuten pitää. Käynnistän demonin uudelleen `sudo systemctl restast salt-minion.service` ja vaihdoin t002 koneelle hyväksyäkseni avaimen `sudo salt-key -A`.

Kun ympäristö oli toiminnassa ja pystyssä, pääsin luomaan omaa moduuliani. Aloitin moduulin luomisen hei maailmalla, jotta näen kaiken toimivan.

Hei maailma-moduuliani varten loin salt-kansioon hello-kansion, johon loin sudoeditillä init.sls tiedoston, jossa seuraavanlainen sisältö:

![image](https://github.com/katariinarytkonen/palvelintenhallinta/assets/164856665/708a7fe5-d79d-4ee1-9ab2-38189d9fd97f)

Seuraavaksi ajoin tilan `sudo salt '*' state.apply hello` -komennolla, ja tarkistin että kaikki toimii:

![image](https://github.com/katariinarytkonen/palvelintenhallinta/assets/164856665/16ab149f-d2b0-4e87-ad3d-d4a89fd90988)

Nyt pääsinkin seuraavaan vaiheeseen.

### Oma moduuli

Loin /srv/saltiin uuden directoryn, omamoduulin. Luon tänne uuden tiedoston ao. sisällöllä `micro init.sls`

![image](https://github.com/katariinarytkonen/palvelintenhallinta/assets/164856665/633a6f48-1c3f-4636-94d0-8114d3818c40)

Lisäksi luon /srv/saltiin top-filen, top.sls johon määrään omamoduulin ajettavaksi ohjelmaksi.

![image](https://github.com/katariinarytkonen/palvelintenhallinta/assets/164856665/5d43aa14-3608-4bc8-bbec-172a9945ec1e)

Testaan tämän toimintaa ensin paikallisesti `sudo salt-call --local state.apply omamoduuli`. 

Tämä toimii, tosin ongelmia tuotti ladattavien pakettien nimet, sillä kaikki ohjelmistot eivät asentuneetkaan ihan niin tarkasti kun olisin toivonut. Testasin libreofficen asennusta, ja se  kesti pienen ikuisuuden, sillä se asensikin monia muitakin ohjelmia, jotka ilmeisesti ovat tämän toiminnalle olennaisia.

Tämä on vähän huono tietysti sen puolesta, että kun asennan nämä orjilleni, kestää tässä taas ikuisuus. Harkitsin että korvaisin thunderbirdin ja libreofficen joillain toisilla ohjelmilla, jotka eivät asentaisi samalla miljoonaa muuta ohjelmaa, sillä nämä ovat vähän huono tälleen demomielessä..
Jätän tämän pohdintaan ja siirryn seuraavaan vaiheeseen.

### Palomuurin asentaminen

Luon uuden directoryn, `/srv/salt/palomuuri`.

Valitsen palumuuriksi ufwn. 

Tänne teen uuden tiedoston init.sls, johon teen palomuurin määrityksiä ja jätän portin 22 auki SSH-yhteyttä varten.

![image](https://github.com/katariinarytkonen/palvelintenhallinta/assets/164856665/1a72df5e-78ed-4592-8512-c3daa7570b50)

Selvästi koodissani oli jotain vikaa, sillä tämä jäi pyörimään taas kymmeniksi minuuteiksi.

![image](https://github.com/katariinarytkonen/palvelintenhallinta/assets/164856665/cd0ee70e-b44b-4878-8957-3f561f57b79d)

Jossain kohtaa painoin enteriä, ja asennus keskeytyi. Mutta näyttäisi kuitenkin toimivan jotenkuten. Tämä näyttää jäävän odottamaan vastausta ufw enablen kohdalla, jonka takia jää pyörimään. 

![image](https://github.com/katariinarytkonen/palvelintenhallinta/assets/164856665/b26a1f51-4587-45ac-9e8e-c6014d1f057b)

Lisään top.sls tiedostooni myös palomuuri-moduulini, ja testaan ajaa tämän ensin paikallisesti, sitten orjilla.

![image](https://github.com/katariinarytkonen/palvelintenhallinta/assets/164856665/4a0c8487-b53d-4fb1-9fcf-e311c360068a)

sudo salt-call --local state.apply
sudo salt '*' state.apply

![image](https://github.com/katariinarytkonen/palvelintenhallinta/assets/164856665/7fbaffdb-da2a-4737-b12e-5f5642bdfca0)

![image](https://github.com/katariinarytkonen/palvelintenhallinta/assets/164856665/e88f515d-f5bc-4c3c-ad18-44921a444b4c)

Tilan ajaminen orjalla ei nyt onnistukaan. Käyn ottamassa moduulieni tiedot talteen, ja pystytän uuden virtuaaliympäristön, sillä olin testannut erinäisiä kill-komentoja eikä mikään näyttänyt nyt toimivan.

### hetkeä myöhemmin

Uudella ympäristöllä ja paikallisesti toimii:

![image](https://github.com/katariinarytkonen/palvelintenhallinta/assets/164856665/ef41a011-c981-4b58-8e37-ca666b7dd4b1)

![image](https://github.com/katariinarytkonen/palvelintenhallinta/assets/164856665/a975f386-71ac-4399-b390-f38a9e13da0b)

Sama ajetaan vielä orjille:  

vagrant@t001:/srv/salt$ sudo salt '*' -l info state.apply 
none:
----------
          ID: omamoduuli
    Function: pkg.installed
      Result: True
     Comment: 3 targeted packages were installed/updated.
     Started: 19:44:30.340227
    Duration: 487122.017 ms
     Changes:   
              ----------
              adwaita-icon-theme:
                  ----------
                  new:
                      3.38.0-1
                  old:
              alsa-topology-conf:
                  ----------
                  new:
                      1.2.4-1
                  old:
              alsa-ucm-conf:
                  ----------
                  new:
                      1.2.4-2
                  old:
              at-spi2-core:
                  ----------
                  new:
                      2.38.0-4+deb11u1
                  old:
              dbus-user-session:
                  ----------
                  new:
                      1.12.28-0+deb11u1
                  old:
              dconf-gsettings-backend:
                  ----------
                  new:
                      0.38.0-2
                  old:
              dconf-service:
                  ----------
                  new:
                      0.38.0-2
                  old:
              firefox-esr:
                  ----------
                  new:
                      115.10.0esr-1~deb11u1
                  old:
              fontconfig:
                  ----------
                  new:
                      2.13.1-4.2
                  old:
              fontconfig-config:
                  ----------
                  new:
                      2.13.1-4.2
                  old:
              fonts-dejavu-core:
                  ----------
                  new:
                      2.37-2
                  old:
              git:
                  ----------
                  new:
                      1:2.30.2-1+deb11u2
                  old:
              git-man:
                  ----------
                  new:
                      1:2.30.2-1+deb11u2
                  old:
              glib-networking:
                  ----------
                  new:
                      2.66.0-2
                  old:
              glib-networking-common:
                  ----------
                  new:
                      2.66.0-2
                  old:
              glib-networking-services:
                  ----------
                  new:
                      2.66.0-2
                  old:
              gsettings-desktop-schemas:
                  ----------
                  new:
                      3.38.0-2
                  old:
              gtk-update-icon-cache:
                  ----------
                  new:
                      3.24.24-4+deb11u3
                  old:
              hicolor-icon-theme:
                  ----------
                  new:
                      0.17-2
                  old:
              i965-va-driver:
                  ----------
                  new:
                      2.4.1+dfsg1-1
                  old:
              intel-media-va-driver:
                  ----------
                  new:
                      21.1.1+dfsg1-1
                  old:
              libaom0:
                  ----------
                  new:
                      1.0.0.errata1-3+deb11u1
                  old:
              libasound2:
                  ----------
                  new:
                      1.2.4-1.1
                  old:
              libasound2-data:
                  ----------
                  new:
                      1.2.4-1.1
                  old:
              libatk-bridge2.0-0:
                  ----------
                  new:
                      2.38.0-1
                  old:
              libatk1.0-0:
                  ----------
                  new:
                      2.36.0-2
                  old:
              libatk1.0-data:
                  ----------
                  new:
                      2.36.0-2
                  old:
              libatspi2.0-0:
                  ----------
                  new:
                      2.38.0-4+deb11u1
                  old:
              libavahi-client3:
                  ----------
                  new:
                      0.8-5+deb11u2
                  old:
              libavahi-common-data:
                  ----------
                  new:
                      0.8-5+deb11u2
                  old:
              libavahi-common3:
                  ----------
                  new:
                      0.8-5+deb11u2
                  old:
              libavcodec58:
                  ----------
                  new:
                      7:4.3.6-0+deb11u1
                  old:
              libavutil56:
                  ----------
                  new:
                      7:4.3.6-0+deb11u1
                  old:
              libcairo-gobject2:
                  ----------
                  new:
                      1.16.0-5
                  old:
              libcairo2:
                  ----------
                  new:
                      1.16.0-5
                  old:
              libcodec2-0.9:
                  ----------
                  new:
                      0.9.2-4
                  old:
              libcolord2:
                  ----------
                  new:
                      1.4.5-3
                  old:
              libcups2:
                  ----------
                  new:
                      2.3.3op2-3+deb11u6
                  old:
              libdatrie1:
                  ----------
                  new:
                      0.2.13-1
                  old:
              libdav1d4:
                  ----------
                  new:
                      0.7.1-3+deb11u1
                  old:
              libdbus-glib-1-2:
                  ----------
                  new:
                      0.110-6
                  old:
              libdconf1:
                  ----------
                  new:
                      0.38.0-2
                  old:
              libdeflate0:
                  ----------
                  new:
                      1.7-1
                  old:
              libdrm-amdgpu1:
                  ----------
                  new:
                      2.4.104-1
                  old:
              libdrm-common:
                  ----------
                  new:
                      2.4.104-1
                  old:
              libdrm-intel1:
                  ----------
                  new:
                      2.4.104-1
                  old:
              libdrm-nouveau2:
                  ----------
                  new:
                      2.4.104-1
                  old:
              libdrm-radeon1:
                  ----------
                  new:
                      2.4.104-1
                  old:
              libdrm2:
                  ----------
                  new:
                      2.4.104-1
                  old:
              libepoxy0:
                  ----------
                  new:
                      1.5.5-1
                  old:
              liberror-perl:
                  ----------
                  new:
                      0.17029-1
                  old:
              libevent-2.1-7:
                  ----------
                  new:
                      2.1.12-stable-1
                  old:
              libfontconfig1:
                  ----------
                  new:
                      2.13.1-4.2
                  old:
              libfribidi0:
                  ----------
                  new:
                      1.0.8-2+deb11u1
                  old:
              libgdk-pixbuf-2.0-0:
                  ----------
                  new:
                      2.42.2+dfsg-1+deb11u1
                  old:
              libgdk-pixbuf2.0-bin:
                  ----------
                  new:
                      2.42.2+dfsg-1+deb11u1
                  old:
              libgdk-pixbuf2.0-common:
                  ----------
                  new:
                      2.42.2+dfsg-1+deb11u1
                  old:
              libgl1:
                  ----------
                  new:
                      1.3.2-1
                  old:
              libgl1-mesa-dri:
                  ----------
                  new:
                      20.3.5-1
                  old:
              libglapi-mesa:
                  ----------
                  new:
                      20.3.5-1
                  old:
              libglvnd0:
                  ----------
                  new:
                      1.3.2-1
                  old:
              libglx-mesa0:
                  ----------
                  new:
                      20.3.5-1
                  old:
              libglx0:
                  ----------
                  new:
                      1.3.2-1
                  old:
              libgomp1:
                  ----------
                  new:
                      10.2.1-6
                  old:
              libgraphite2-3:
                  ----------
                  new:
                      1.3.14-1
                  old:
              libgsm1:
                  ----------
                  new:
                      1.0.18-2
                  old:
              libgtk-3-0:
                  ----------
                  new:
                      3.24.24-4+deb11u3
                  old:
              libgtk-3-bin:
                  ----------
                  new:
                      3.24.24-4+deb11u3
                  old:
              libgtk-3-common:
                  ----------
                  new:
                      3.24.24-4+deb11u3
                  old:
              libharfbuzz0b:
                  ----------
                  new:
                      2.7.4-1
                  old:
              libice6:
                  ----------
                  new:
                      2:1.0.10-1
                  old:
              libigdgmm11:
                  ----------
                  new:
                      20.4.1+ds1-1
                  old:
              libjbig0:
                  ----------
                  new:
                      2.1-3.1+b2
                  old:
              libjpeg62-turbo:
                  ----------
                  new:
                      1:2.0.6-4
                  old:
              libjson-glib-1.0-0:
                  ----------
                  new:
                      1.6.2-1
                  old:
              libjson-glib-1.0-common:
                  ----------
                  new:
                      1.6.2-1
                  old:
              liblcms2-2:
                  ----------
                  new:
                      2.12~rc1-2
                  old:
              libllvm11:
                  ----------
                  new:
                      1:11.0.1-2
                  old:
              libmfx1:
                  ----------
                  new:
                      21.1.0-1
                  old:
              libmp3lame0:
                  ----------
                  new:
                      3.100-3
                  old:
              libnuma1:
                  ----------
                  new:
                      2.0.12-1+b1
                  old:
              libogg0:
                  ----------
                  new:
                      1.3.4-0.1
                  old:
              libopenjp2-7:
                  ----------
                  new:
                      2.4.0-3
                  old:
              libopus0:
                  ----------
                  new:
                      1.3.1-0.1
                  old:
              libpango-1.0-0:
                  ----------
                  new:
                      1.46.2-3
                  old:
              libpangocairo-1.0-0:
                  ----------
                  new:
                      1.46.2-3
                  old:
              libpangoft2-1.0-0:
                  ----------
                  new:
                      1.46.2-3
                  old:
              libpciaccess0:
                  ----------
                  new:
                      0.16-1
                  old:
              libpixman-1-0:
                  ----------
                  new:
                      0.40.0-1.1~deb11u1
                  old:
              libproxy1v5:
                  ----------
                  new:
                      0.4.17-1
                  old:
              librest-0.7-0:
                  ----------
                  new:
                      0.8.1-1.1
                  old:
              librsvg2-2:
                  ----------
                  new:
                      2.50.3+dfsg-1+deb11u1
                  old:
              librsvg2-common:
                  ----------
                  new:
                      2.50.3+dfsg-1+deb11u1
                  old:
              libsensors-config:
                  ----------
                  new:
                      1:3.6.0-7
                  old:
              libsensors5:
                  ----------
                  new:
                      1:3.6.0-7
                  old:
              libshine3:
                  ----------
                  new:
                      3.1.1-2
                  old:
              libsm6:
                  ----------
                  new:
                      2:1.2.3-1
                  old:
              libsnappy1v5:
                  ----------
                  new:
                      1.1.8-1
                  old:
              libsoup-gnome2.4-1:
                  ----------
                  new:
                      2.72.0-2
                  old:
              libsoup2.4-1:
                  ----------
                  new:
                      2.72.0-2
                  old:
              libsoxr0:
                  ----------
                  new:
                      0.1.3-4
                  old:
              libspeex1:
                  ----------
                  new:
                      1.2~rc1.2-1.1
                  old:
              libswresample3:
                  ----------
                  new:
                      7:4.3.6-0+deb11u1
                  old:
              libthai-data:
                  ----------
                  new:
                      0.1.28-3
                  old:
              libthai0:
                  ----------
                  new:
                      0.1.28-3
                  old:
              libtheora0:
                  ----------
                  new:
                      1.1.1+dfsg.1-15
                  old:
              libtiff5:
                  ----------
                  new:
                      4.2.0-1+deb11u5
                  old:
              libtwolame0:
                  ----------
                  new:
                      0.4.0-2
                  old:
              libva-drm2:
                  ----------
                  new:
                      2.10.0-1
                  old:
              libva-x11-2:
                  ----------
                  new:
                      2.10.0-1
                  old:
              libva2:
                  ----------
                  new:
                      2.10.0-1
                  old:
              libvdpau-va-gl1:
                  ----------
                  new:
                      0.4.2-1+b1
                  old:
              libvdpau1:
                  ----------
                  new:
                      1.4-3
                  old:
              libvorbis0a:
                  ----------
                  new:
                      1.3.7-1
                  old:
              libvorbisenc2:
                  ----------
                  new:
                      1.3.7-1
                  old:
              libvpx6:
                  ----------
                  new:
                      1.9.0-1+deb11u2
                  old:
              libvulkan1:
                  ----------
                  new:
                      1.2.162.0-1
                  old:
              libwavpack1:
                  ----------
                  new:
                      5.4.0-1
                  old:
              libwayland-client0:
                  ----------
                  new:
                      1.18.0-2~exp1.1
                  old:
              libwayland-cursor0:
                  ----------
                  new:
                      1.18.0-2~exp1.1
                  old:
              libwayland-egl1:
                  ----------
                  new:
                      1.18.0-2~exp1.1
                  old:
              libwebp6:
                  ----------
                  new:
                      0.6.1-2.1+deb11u2
                  old:
              libwebpmux3:
                  ----------
                  new:
                      0.6.1-2.1+deb11u2
                  old:
              libx11-6:
                  ----------
                  new:
                      2:1.7.2-1+deb11u2
                  old:
              libx11-data:
                  ----------
                  new:
                      2:1.7.2-1+deb11u2
                  old:
              libx11-xcb1:
                  ----------
                  new:
                      2:1.7.2-1+deb11u2
                  old:
              libx264-160:
                  ----------
                  new:
                      2:0.160.3011+gitcde9a93-2.1
                  old:
              libx265-192:
                  ----------
                  new:
                      3.4-2
                  old:
              libxau6:
                  ----------
                  new:
                      1:1.0.9-1
                  old:
              libxcb-dri2-0:
                  ----------
                  new:
                      1.14-3
                  old:
              libxcb-dri3-0:
                  ----------
                  new:
                      1.14-3
                  old:
              libxcb-glx0:
                  ----------
                  new:
                      1.14-3
                  old:
              libxcb-present0:
                  ----------
                  new:
                      1.14-3
                  old:
              libxcb-randr0:
                  ----------
                  new:
                      1.14-3
                  old:
              libxcb-render0:
                  ----------
                  new:
                      1.14-3
                  old:
              libxcb-shm0:
                  ----------
                  new:
                      1.14-3
                  old:
              libxcb-sync1:
                  ----------
                  new:
                      1.14-3
                  old:
              libxcb-xfixes0:
                  ----------
                  new:
                      1.14-3
                  old:
              libxcb1:
                  ----------
                  new:
                      1.14-3
                  old:
              libxcomposite1:
                  ----------
                  new:
                      1:0.4.5-1
                  old:
              libxcursor1:
                  ----------
                  new:
                      1:1.2.0-2
                  old:
              libxdamage1:
                  ----------
                  new:
                      1:1.1.5-2
                  old:
              libxdmcp6:
                  ----------
                  new:
                      1:1.1.2-3
                  old:
              libxext6:
                  ----------
                  new:
                      2:1.3.3-1.1
                  old:
              libxfixes3:
                  ----------
                  new:
                      1:5.0.3-2
                  old:
              libxi6:
                  ----------
                  new:
                      2:1.7.10-1
                  old:
              libxinerama1:
                  ----------
                  new:
                      2:1.1.4-2
                  old:
              libxkbcommon0:
                  ----------
                  new:
                      1.0.3-2
                  old:
              libxmu6:
                  ----------
                  new:
                      2:1.1.2-2+b3
                  old:
              libxmuu1:
                  ----------
                  new:
                      2:1.1.2-2+b3
                  old:
              libxrandr2:
                  ----------
                  new:
                      2:1.5.1-1
                  old:
              libxrender1:
                  ----------
                  new:
                      1:0.9.10-1
                  old:
              libxshmfence1:
                  ----------
                  new:
                      1.3-1
                  old:
              libxt6:
                  ----------
                  new:
                      1:1.2.0-1
                  old:
              libxtst6:
                  ----------
                  new:
                      2:1.2.3-1
                  old:
              libxvidcore4:
                  ----------
                  new:
                      2:1.3.7-1
                  old:
              libxxf86vm1:
                  ----------
                  new:
                      1:1.1.4-1+b2
                  old:
              libz3-4:
                  ----------
                  new:
                      4.8.10-1
                  old:
              libzvbi-common:
                  ----------
                  new:
                      0.2.35-18
                  old:
              libzvbi0:
                  ----------
                  new:
                      0.2.35-18
                  old:
              mesa-va-drivers:
                  ----------
                  new:
                      20.3.5-1
                  old:
              mesa-vdpau-drivers:
                  ----------
                  new:
                      20.3.5-1
                  old:
              mesa-vulkan-drivers:
                  ----------
                  new:
                      20.3.5-1
                  old:
              micro:
                  ----------
                  new:
                      2.0.8-1+b6
                  old:
              ocl-icd-libopencl1:
                  ----------
                  new:
                      2.2.14-2
                  old:
              patch:
                  ----------
                  new:
                      2.7.6-7
                  old:
              shared-mime-info:
                  ----------
                  new:
                      2.0-1
                  old:
              va-driver-all:
                  ----------
                  new:
                      2.10.0-1
                  old:
              vdpau-driver-all:
                  ----------
                  new:
                      1.4-3
                  old:
              x11-common:
                  ----------
                  new:
                      1:7.7+22
                  old:
              xauth:
                  ----------
                  new:
                      1:1.1-1
                  old:
              xclip:
                  ----------
                  new:
                      0.13-2
                  old:
              xkb-data:
                  ----------
                  new:
                      2.29-2
                  old:
----------
          ID: sudo apt-get install ufw
    Function: cmd.run
      Result: True
     Comment: Command "sudo apt-get install ufw" run
     Started: 19:52:37.539886
    Duration: 17453.37 ms
     Changes:   
              ----------
              pid:
                  6630
              retcode:
                  0
              stderr:
                  debconf: unable to initialize frontend: Dialog
                  debconf: (Dialog frontend will not work on a dumb terminal, an emacs shell buffer, or without a controlling terminal.)
                  debconf: falling back to frontend: Readline
                  debconf: unable to initialize frontend: Readline
                  debconf: (This frontend requires a controlling tty.)
                  debconf: falling back to frontend: Teletype
                  dpkg-preconfigure: unable to re-open stdin:
              stdout:
                  Reading package lists...
                  Building dependency tree...
                  Reading state information...
                  The following NEW packages will be installed:
                    ufw
                  0 upgraded, 1 newly installed, 0 to remove and 21 not upgraded.
                  Need to get 167 kB of archives.
                  After this operation, 857 kB of additional disk space will be used.
                  Get:1 https://deb.debian.org/debian bullseye/main amd64 ufw all 0.36-7.1 [167 kB]
                  Fetched 167 kB in 0s (2389 kB/s)
                  Selecting previously unselected package ufw.
                  (Reading database ... 
                  (Reading database ... 5%
                  (Reading database ... 10%
                  (Reading database ... 15%
                  (Reading database ... 20%
                  (Reading database ... 25%
                  (Reading database ... 30%
                  (Reading database ... 35%
                  (Reading database ... 40%
                  (Reading database ... 45%
                  (Reading database ... 50%
                  (Reading database ... 55%
                  (Reading database ... 60%
                  (Reading database ... 65%
                  (Reading database ... 70%
                  (Reading database ... 75%
                  (Reading database ... 80%
                  (Reading database ... 85%
                  (Reading database ... 90%
                  (Reading database ... 95%
                  (Reading database ... 100%
                  (Reading database ... 39087 files and directories currently installed.)
                  Preparing to unpack .../archives/ufw_0.36-7.1_all.deb ...
                  Unpacking ufw (0.36-7.1) ...
                  Setting up ufw (0.36-7.1) ...
                  debconf: unable to initialize frontend: Dialog
                  debconf: (Dialog frontend will not work on a dumb terminal, an emacs shell buffer, or without a controlling terminal.)
                  debconf: falling back to frontend: Readline
                  
                  Creating config file /etc/ufw/before.rules with new version
                  
                  Creating config file /etc/ufw/before6.rules with new version
                  
                  Creating config file /etc/ufw/after.rules with new version
                  
                  Creating config file /etc/ufw/after6.rules with new version
                  Created symlink /etc/systemd/system/multi-user.target.wants/ufw.service -> /lib/systemd/system/ufw.service.
                  Processing triggers for rsyslog (8.2102.0-2+deb11u1) ...
                  Processing triggers for man-db (2.9.4-2) ...
----------
          ID: ufw allow 22/tcp
    Function: cmd.run
      Result: True
     Comment: Command "ufw allow 22/tcp" run
     Started: 19:52:54.993666
    Duration: 2357.654 ms
     Changes:   
              ----------
              pid:
                  7077
              retcode:
                  0
              stderr:
              stdout:
                  Rules updated
                  Rules updated (v6)
----------
          ID: ufw enable
    Function: cmd.run
      Result: True
     Comment: Command "ufw enable" run
     Started: 19:52:57.351758
    Duration: 736.827 ms
     Changes:   
              ----------
              pid:
                  7100
              retcode:
                  0
              stderr:
              stdout:
                  Firewall is active and enabled on system startup

Summary for none
------------
Succeeded: 4 (changed=4)
Failed:    0
------------
Total states run:     4
Total run time: 507.670 s

![image](https://github.com/katariinarytkonen/palvelintenhallinta/assets/164856665/ad52465c-6bd6-44b3-b74e-635138861036)

Näyttää asentuneen.

Tarkistin vielä että luvatut on myös tehty.

![image](https://github.com/katariinarytkonen/palvelintenhallinta/assets/164856665/64ce758d-4278-4ecd-bea9-2cdfbefcdec7)

Jes, kaikki toimi ja näin voisin asentaa vaikka kuinka monelle koneelle samat ohjelmat ja samat säädöt kerralla. 

### Lähteet

Karvinen 2024: Configuration management 2024 spring. Luettavissa:

https://terokarvinen.com/2024/configuration-management-2024-spring/. Luettu 12.5.2024

Karvinen 2021: Two Machine Virtual Network With Debian 11 Bullseye and Vagrant. Luettavissa:

https://terokarvinen.com/2021/two-machine-virtual-network-with-debian-11-bullseye-and-vagrant/ Luettu 12.5.2024

Karvinen 2016: Instant Firewall Sudo Ufw Enable. Luettavissa:

https://terokarvinen.com/2016/instant-firewall-sudo-ufw-enable/?fromSearch=firewall Luettu 12.5.2024

https://github.com/katariinarytkonen/palvelintenhallinta/blob/main/H2%20Soitto%20Kotiin.md Luettu 12.5.2024

https://wiki.debian.org/Firefox Luettu 12.5.2024
