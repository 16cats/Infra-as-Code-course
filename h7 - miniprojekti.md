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


Muokataan seuraavaksi Vagrantfileä. `notepad Vagrantfile`

![image](https://github.com/16cats/Infra-as-Code-course/assets/97065659/2c60da67-82a9-4b88-8ce5-cc2c28a27b13)

`vagrant up` & `vagrant ssh master`:

`sudo salt-key -A`:

![image](https://github.com/16cats/Infra-as-Code-course/assets/97065659/7ed5ca92-039a-4249-817e-5410161ecfef)

Onko yhteys luotu onnistuneesti?

![image](https://github.com/16cats/Infra-as-Code-course/assets/97065659/059755e5-bb2a-4751-aafc-b80e934662ca)


## VSCoden sekä Python-laajennuksen asennus

![image](https://github.com/16cats/Infra-as-Code-course/assets/97065659/09d4a0b6-adde-4d1e-b0ac-79e810e34e7d)

Ajattelin lataa snapd:n avulla kun ajattelin, että se olisi helpompaa niin.

![image](https://github.com/16cats/Infra-as-Code-course/assets/97065659/10615395-c17c-4387-bec3-da8da1e6bbe2)

![image](https://github.com/16cats/Infra-as-Code-course/assets/97065659/807a1f3c-3302-480b-a779-83527c8a5738)

Taisin unohtaa pistää kansion /srv/salt/init.sls hakemistoon. Korjataan tämä. Olisin voinut sen siirtää, mutta loin vain uuden tekstitiedoston /srv/salt/:iin. Sitten ajetaan `sudo salt 's001' state.apply`

Sain sitten vscoden ladattua, mutta en 

![image](https://github.com/16cats/Infra-as-Code-course/assets/97065659/3730f14c-e784-4920-9392-c2cb1bdfa463)

![image](https://github.com/16cats/Infra-as-Code-course/assets/97065659/b0b39497-8f10-45db-8ccd-dd8fecf78c1f)

