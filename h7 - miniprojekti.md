# Kotitehtävä 7: Miniprojekti

## Tarkoitus

Miniprojektin/ oman modulin ohjeena on luoda jokin moduli helpottamaan jotain toiminnallisuutta elämässä.
Miniprojektissani on tarkoituksena ladata minioneille automaattisesti VSCode sekä samassa paketissa Python-laajennus VSCodeen.

Aloitin luomalla uuden vagrantin, masterin ja minionit. Hyödynsin paljon edellisiä raporttejani sekä [Karvisen](https://terokarvinen.com/2023/configuration-management-2023-autumn/) ohjeistuksia ja dokumentteja. 


`vagrant box add debian/bullseye64` & `vagrant init debian/bullseye64`

![image](https://github.com/16cats/Infra-as-Code-course/assets/97065659/640ef6b6-3435-46b7-8767-8bd521fb4104)

Käynnistetään Vagrant ja luodaan siihen SSH-yhteys.

`vagrant up & vagrant ssh:`

![image](https://github.com/16cats/Infra-as-Code-course/assets/97065659/2de7b31c-e681-476b-bcd7-838602b7a159)

![image](https://github.com/16cats/Infra-as-Code-course/assets/97065659/0098f2d7-92f7-47c2-b6f2-509b3e8a3b81)


`sudo apt-get install salt-master` ei heti toiminut, niin tajusin päivittää ensin ( `sudo apt-get update` & `sudo apt-get upgrade` ). Sitten laitoin masterin ja minionin (`sudo apt-get install salt-master` & `sudo apt-get install salt-minion`) lataukseen.

`notepad Vagrantfile`: Muokataan seuraavaksi Vagrantfileä. Kopioin sinne valmiin [valmiin Vagrantfilen](https://terokarvinen.com/2023/salt-vagrant/)

![image](https://github.com/16cats/Infra-as-Code-course/assets/97065659/a0ace446-4a6b-4506-855b-27a900975fd3)


`vagrant up` & `vagrant ssh master`:

![image](https://github.com/16cats/Infra-as-Code-course/assets/97065659/2c60da67-82a9-4b88-8ce5-cc2c28a27b13)

`sudo salt-key -A`, hyväksytään avaimet.

![image](https://github.com/16cats/Infra-as-Code-course/assets/97065659/7ed5ca92-039a-4249-817e-5410161ecfef)

Onko yhteys luotu onnistuneesti?

![image](https://github.com/16cats/Infra-as-Code-course/assets/97065659/059755e5-bb2a-4751-aafc-b80e934662ca)


## VSCoden sekä Python-laajennuksen asennus

Seuraavaksi olisi tarkoituksena asettaa minioneille VSCode ja Python-laajennus.

![image](https://github.com/16cats/Infra-as-Code-course/assets/97065659/09d4a0b6-adde-4d1e-b0ac-79e810e34e7d)

Ajattelin lataa snapd:n avulla kun ajattelin, että se olisi helpompaa niin. 

(13.12.2023 note: Snap on pakettienhallinta, jota käytin ehkä joskus 3 vuotta sitten. En tosiaan suosittele käyttämään sitä henkilökohtasesti turvariskien takia, mutta ajattelin, että sitä olisi helppo käyttää semi-lyhkäsiin komentoihin.)

![image](https://github.com/16cats/Infra-as-Code-course/assets/97065659/10615395-c17c-4387-bec3-da8da1e6bbe2)

![image](https://github.com/16cats/Infra-as-Code-course/assets/97065659/807a1f3c-3302-480b-a779-83527c8a5738)

Taisin unohtaa pistää kansion /srv/salt/init.sls hakemistoon. Korjataan tämä. Olisin voinut sen siirtää, mutta loin vain uuden tekstitiedoston /srv/salt/:iin. Sitten ajetaan `sudo salt 's001' state.apply`

![image](https://github.com/16cats/Infra-as-Code-course/assets/97065659/b0b39497-8f10-45db-8ccd-dd8fecf78c1f)

![image](https://github.com/16cats/Infra-as-Code-course/assets/97065659/3730f14c-e784-4920-9392-c2cb1bdfa463)

Sain kai sitten VSCoden ladattua, mutta en Python-laajennusta. En hahmota minkä takia.

(13.12.2023 note: en sittenkään usko, että onnistuin snapilla asentaa VSCodea, vaikka niin se ilmoittaa. Kokeilin tehdä sitä käsin jälkeenpäin, eikä mielestäni codea oltu asennettu, vaikka ilmoittaakin et jokin on ladattu.)

Yritin vielä moneen otteeseen testata eri tapoja ladata VSCodea. 

Lopulta päätin, että järkevintä on lähteä tekemään projektia ensin käsin. Katsoin ohjeet [virallisilta sivuilta](https://code.visualstudio.com/docs/setup/linux) ja lähdin lataamaan tätä Virtual Boxissa Debianilla.

```
sudo apt-get install wget gpg
wget -qO- https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > packages.microsoft.gpg
sudo install -D -o root -g root -m 644 packages.microsoft.gpg /etc/apt/keyrings/packages.microsoft.gpg
sudo sh -c 'echo "deb [arch=amd64,arm64,armhf signed-by=/etc/apt/keyrings/packages.microsoft.gpg] https://packages.microsoft.com/repos/code stable main" > /etc/apt/sources.list.d/vscode.list'
rm -f packages.microsoft.gpg
```

```
sudo apt install apt-transport-https
sudo apt update
sudo apt install code # or code-insiders
```

![image](https://github.com/16cats/Infra-as-Code-course/assets/97065659/0124eb8c-ed92-4d10-85e6-64eda3868773)

![image](https://github.com/16cats/Infra-as-Code-course/assets/97065659/c27216e5-c4b9-4234-8602-5b9750a64c71)

Ja tämä lataus meni ihan sulavasti.

![image](https://github.com/16cats/Infra-as-Code-course/assets/97065659/86cb09ad-d59a-406f-8997-10fe5386bd19)

Yritin sitten implementoida tätä latausta Salttiin, mutta en millään onnistunut. Tuijottelin ja yritin etsiä tietoa miten saisin sen toimimaan Saltissa. Monia eri erroreja tuli vastaan ja valitettavasti aikakin alkoi loppumaan.

![image](https://github.com/16cats/Infra-as-Code-course/assets/97065659/da893280-df63-44e0-b86f-0475ea08a309)

Harmittaa toki :) .
