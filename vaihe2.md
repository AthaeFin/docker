# docker-compose, joka luo kontit portainerille, tietokannalle, ja sitä käyttävälle WordPressille
Portainer on ohjelma, jota käytetään konttien hallintaan, ylläpitoon, siirtämiseen palvelimelta toiselle jne. Portainer on käytössä yritysmaailmassa,
saltin sijaan olen kuullut käytettävän Ansiblea.

Docker-compose on yaml-pohjainen tiedosto, joka toimii hyvin samalla tavalla kuin edellisen vaiheen init.sls. Määritellään mitä halutaan tehdä, ja annetaan luomiskäsky.
(tulkoon kontti, ja kontti tuli). 

Muistetaan edellisestä tehtävästä, että Docker tarvitsee root-käyttäjän sitä koskeviin tiedostoihin, joten luulen tämän olevan syy, miksi sudoedit ei toimi: 

![image](https://github.com/user-attachments/assets/538ec33b-5c69-43e8-a7fb-aba4c1064731)

    sudo nano docker-compose.yaml

.yaml-tiedostoissa kuten myös .sls-tiedostoissa, ei käytetä tabulaattoria, vaan välilyöntiä. Monesti tiedoston kirjoittaminen menee reisille, kun esimerkiksi
ports ja volumes ei ole samassa kohdassa listattuna, esimerkiksi liikaa välilyöntejä. Alla oleva teksti luo kolme levytilaa kolmelle kontille, määrittelee portit ja
ladattavat ISO-tiedostot. Sama tiedosto ilman portaineria on saatavilla Dockerin omilta sivuilta https://hub.docker.com/_/wordpress, tietenkin itse täytyy määritellä salasanat ja käyttäjät. Jos haluaa
ulkoiset levytilat tiedon säilymisen takaamiseksi, voi määritellä levytilan external: true.

    services:
    
      portainer:
        image: portainer/portainer-ce:latest
        restart: unless-stopped
        ports:
          - 9443:9443
        volumes:
          - data:/data
          - /var/run/docker.sock:/var/run/docker.sock
    
      wordpress:
        image: wordpress
        restart: always
        ports:
          - 8080:80
        environment:
          WORDPRESS_DB_HOST: db
          WORDPRESS_DB_USER: wordpress
          WORDPRESS_DB_PASSWORD: kilpakonna234
          WORDPRESS_DB_NAME: wordpress
        volumes:
          - wordpress:/var/www/html
    
      db:
        image: mysql:8.0
        restart: always
        environment:
          MYSQL_DATABASE: wordpress
          MYSQL_USER: wordpress
          MYSQL_PASSWORD: kilpakonna234
          MYSQL_RANDOM_ROOT_PASSWORD: '1'
        volumes:
          - db:/var/lib/mysql
    
    volumes:
     data:
     wordpress:
     db:

Ja hyvä huomio, että kannattaa tosiaan kirjoittaa .yaml-tiedosto ennen luontia:

![image](https://github.com/user-attachments/assets/4c59f9b5-0723-498d-b187-bb509219a7b9)

    sudo docker-compose up -d

Ja nyt meillä on 3 konttia kalastusveneessä.... paitsi jos jäätiin kanavaan jumiin, kuten itselle tätä dokumentaatiota tehdessä. Liian hätäinen ei kannata olla,
pääsin hetken odottelulla kanavassa taas eteenpäin. 

    sudo docker ps
    
![image](https://github.com/user-attachments/assets/ab2f1278-955e-4fca-b519-af8662e46a8c)

ja sitten käynnistetään minion uudelleen:

![image](https://github.com/user-attachments/assets/52de6385-8b80-4054-b19f-38234c267f04)

Uudelleenkäynnistyksen jälkeen meillä on Portainer osoitteessa https://10.20.30.101:9443 ja 
WordPress http://10.20.30.101:8080

![image](https://github.com/user-attachments/assets/080222a7-06e9-4978-b211-0bdfbe788467)


![image](https://github.com/user-attachments/assets/d7816dab-ff99-49e4-9870-3c8753460d1a)


