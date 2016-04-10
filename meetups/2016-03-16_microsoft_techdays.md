# Microsoft Tech Days

https://github.com/microsoft

## F5 Network
- Specialisé dans la sécurité dans le Cloud

## Trends/tendances UX de 2016 - @rousseau_michel
- Utiliser la meme experience sur toutes les plateformes
- Card style : rapport émotif qui va nous permettre d'avoir des visuels sympathiques, modulaires. Notion du contenu.
- Le design "télépathique" : design qui n'a pas d'interface. e.g. Google Soli
- Le motion design vidéo : call to action + efficaces, impact visuel beaucoup plus fort

## Applications

### Hybride
- Apache Cordova : manifoldjs - transforme  un site web existant en web app
- HTML + JS ---> manifoldjs ---> windows phone, ios, android
-
### Natives
- Visual Studio C++ cross-platform mobile
- Xamarin
- Unity : http://aka.ms/devhololens
- Visual Studio Code

## Azure
- https://www.github.com/Azure/azure-quickstart-templates
- https://www.github.com/herveleclerc/TechDaysCampDemo
- armviz.io

## Web
- lewebmoderne.com
- dev.windows.com/en-us/microsoft-edge/tools/staticscan (aka.ms/webscan)
- vorlon.js - onglet best practices (vorlonjs.io)

## Docker Workshop
- busybox = mini linux

### Conteneur nginx
- docker run --rm -it nginx
- docker run --rm -it -p 8080:80 nginx
- PORT sur ma machine est mappé vers le PORT 80 du conteneur (sur lequel tourne nginx)

### Supprimer tous les conteneurs
- docker rm -f $(docker ps -aq)

### Permet de créer des machines sur le cloud ou en local
- docker-machine create -d virtualbox toto

### Construire des images de contenurs via un Dockerfile
- Dockerfile
- docker build -t test .

FROM ubuntu:latest
RUN apt-get update && apt-get install -y git
CMD ["git", "--version"]

-
### Monter un répertoire de ma mchine directement à l'intérieur du conteneur
- docker run --rm busybox ls /data
- docker run --rm -v /Users/jonathan/:/data busybox ls /data
- Un conteneur est capable d'écrire dans des fichiers se trouvant à l'extérieur du conteneur

### Docker Swarm
- Permet de créer des clusters
- Cluster de 100 machines, on veut déployer 5 microservices. Docker swarm va créer les conteneurs sur les machines de manière performante

### Docker compose
docker-compose scale checkout=3

docker-compose up
docker-compose up -d (mode daemon)

docker-compose down

github.com/ehazlett/interlock



docker-swam use ...
docker-compose ps

docker-compose down
docker-compose up

docker-compose stop <name>
docker-compose build <name>
docker-compose start <name>
docker-compose logs <name>

github.com/DXFrance/MusicStore
