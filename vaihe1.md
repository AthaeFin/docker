# Vagrant, Salt-Master ja -Minion

Edellisiä tehtäviä mukaillen käytin samaa vagrantfile-tiedostoa, jota aikaisemminkin on käytetty masterin ja minionin kanssa, paitsi muistia oli lisättävä: konttien
kanssa olin sellaisessa tilanteessa, että muisti ei riittänyt, jolloin WordPressin käyttämä tietokanta pysyi käynnissä muutaman sekuntin ja sitten kyykkäsi, jolloin
myöskään Wordpress ei käynnistynyt selaimessa. Tilannetta voi ajatella, että minulla oli kassakaappi kumiveneessä. Vagrantfile -tiedostoon lisäämäni rivi kumminkin
lisäsi n. 8-kertaisesti kummankin virtuaalikoneen muistia ja prosessoritehoa 4-kertaisesti, jolloin kumiveneestä tuli pieni kalastusvene, jossa moottori. 

<-- Koko vagrantfile löytyy vasemmalta:

Lisärivi:

    config.vm.provider "virtualbox" do |v|
      v.memory = 4096
      v.cpus = 4
    end

Taas lähdin komennolla vagrant up, jolloin vagrant loi minulle kaksi virtuaalikonetta. Ensin kirjauduin SSH-yhteydellä master-koneelle, hyväksyin minionin avaimen, 
loin hakemiston Dockerille ja hakemistossa loin init.sls -tiedoston:

    sudo salt-key -A
    sudo mkdir -p /srv/salt/docker
    cd /srv/salt/docker
    sudoedit init.sls

![image](https://github.com/user-attachments/assets/349a4524-c839-4f13-a1cf-b3165791622b)

![image](https://github.com/user-attachments/assets/1c9546d5-8d1c-4a48-b24f-426b71e1d87e)

init-tiedostossa ensin määritellään riippuvuudet, nyt jälkeenpäin miettien saattaa olla turha, koska tiedetään, että curl on jo asennettu vagrantfilessä, mutta taitaa
olla ca-certificates samalla. Katson asian seuraavassa vaiheessa, mitä salt asiasta sanoo*. Docker-key ja docker repository ovat samantapaisia kuin saltin keyring ja
repository. 
Dockerilla täytyy 664 -oikeudet, jotta vain root-käyttäjä voi muokata sen tiedostoja. Lähde: https://docs.datadoghq.com/security/default_rules/6ia-vhn-rrf/

Ylläolevassa init.sls -tiedostossa näkyy, kuinka seuraavaa vaihetta ei toteuteta, ellei edellä listatut asiat toteudu require:n avulla. 
Lopuksi määritellään vielä Dockerin tarvitsemat lisäpalikat, tärkeimpänä docker-compose, koska sitä käytetään seuraavassa vaiheessa. 

Sen jälkeen pistin dockerin pystyyn:

    sudo salt '*' state.apply docker


* Testitulos:
  
![image](https://github.com/user-attachments/assets/0eb1cf59-780f-4072-a596-3a484df21bb7)

eli init.sls -tiedostoon voi listata ca-certificates ja curl riippuvuuksiksi, tai ottaa koko rimpsun pois ja ottaa keyrings-kohdassa require: riippuvuudet pois.

![image](https://github.com/user-attachments/assets/365bfd8b-d413-4e7e-90d8-19d537b12b9b)

![image](https://github.com/user-attachments/assets/44583c60-4107-4d53-9b99-0d5a121181ae)

![image](https://github.com/user-attachments/assets/9d89ca8d-d4ca-4eed-ac7d-1c5bb7d164d9)

![image](https://github.com/user-attachments/assets/990ba0df-2fa9-4be4-8c52-b7851b4731e3)

Succeeded: 10 (changed=4) kertoo, että turhaa tavaraa init.sls -tiedostossa on vielä, perkaan pois jos aikaa jää jäljelle. Miesvaiva idempotentti siis toimii
ja kalastusvene on valmiina ottamaan kontteja vastaan, toivotaan että ei mene poikittain kanavaan jumiin :)










    
