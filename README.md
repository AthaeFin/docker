# Alussa olivat Salt, Docker - ja Portainer


salt master ja minion, docker, portainer, wp ja db muutamalla potkaisukomennolla

## Miniprojektin alku

Pähkäilin pitkään, mitä lähtisin palvelinten hallinta -kurssilla tekemään projektiksi, ja lopulta päädyin samaan projektiin, jonka tein Linux-serverit -kurssilla, pienellä twistillä. Tavoitteena on saavuttaa sama lopputulos kuin edellisessä projektissa, mutta toteuttaa se muutamalla compose-komennolla. Salt (Master) asentaa Dockerin init.sls -tiedoston avulla Minion-koneelle, ja sen jälkeen minionilla Docker asentaa Portainerin, WordPressin ja sen käyttämän tietokannan. 

Toisinsanoen halusin löytää keinon, jolla edellisen kurssin projektin voi hoitaa 'laiskasti', monta kertaa toistettavasti, ja jossa ihmisen virheen mahdollisuus jää pieneksi.

Aloita vaiheesta 1, ja jatka vaiheeseen 2. Tarvittavat konfigurointitiedostot löytyvät vasemmalta niiden omalla nimellä. 
Koko projekti on testattu omalla koneella toimivasti 3 kertaa alusta loppuun. Kaikkia kirjoitusvirheitä en ole kirjannut, mutta suurimmat kompastuskivet olen maininnut. 

Koko projektiin meni aikaa n. 16 tuntia mukaanlukien dokumentointi. 



lähteet:

Tero Karvinen 2025, h5 miniprojekti. Luettavissa:

https://terokarvinen.com/palvelinten-hallinta/#h5-miniprojekti. Luettu 1.5.2025


Tero Karvinen 2023, infra as code. Luettavissa:

https://terokarvinen.com/2023/salt-vagrant/#infra-as-code---your-wishes-as-a-text-file. Luettu 4.5.2025


James Walker 2023, Using Portainer with Docker and Docker compose, Luettavissa:

https://earthly.dev/blog/portainer-for-docker-container-management/. Luettu 4.5.2025


Hashicorp, virtualbox configuration. Luettavissa:

https://developer.hashicorp.com/vagrant/docs/providers/virtualbox/configuration. Luettu 4.5.2025


Sai Kiram Pikili 2024, Automating Docker installation with SaltStack. Luettavissa:

https://saikiranpikili.medium.com/building-your-monitoring-stack-automating-docker-installation-with-saltstack-9d352d8e14ad. Luettu 4.5.2025


The Docker Community 2025, WordPress. Luettavissa:

https://hub.docker.com/_/wordpress. Luettu 4.5.2025
