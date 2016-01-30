---
layout: post
title:  "Mise en place d'un serveur Node.js avec Docker"
date:   2015-12-25 19:45:00
categories: sysadmin
banner: linux.png
---
Dans ce petit guide nous allons créer un serveur Node.js et le faire tourner grace à l'image Docker officielle.

Dans un premier temps nous créons notre projet :
{% highlight bash %}
mkdir node_serveur_project && touch $_/server.js
{% endhighlight %}

Le fichier __node_serveur_project/server.js__ doit contenir un serveur tel que :
{% highlight javascript %}
var http = require('http');

http.createServer(function (request, response) {
  response.writeHead(200, {'Content-Type': 'text/plain'});
  response.end('Hello World\n');
}).listen(8080);

console.log('Server running at http://127.0.0.1:8080/');
{% endhighlight %}

Notre projet est maintenant terminé, nous voulons l'installer sur un serveur.

> Avant de continuer, assurez vous d'avoir Docker installé sur votre machine.

Rendons-nous dans le dossier node_serveur_project et connectons nous à un container grace à la commande suivante :

{% highlight bash %}
docker run --rm -v $PWD:/srv -w /srv -p 80:8080 -ti node /bin/bash
{% endhighlight %}

Description de la commande :    
* __docker run__ : permet de lancer une commande dans un container
* __--rm__ : permet de suprimer le container à la fin de l'utilisation
* __-v $PWD:/srv__ : monte le repertoire courant dans le dossier /srv du container
* __-w /srv__ : indique le dossier de travail dans le container
* __-p 80:8080__ : lie le port 8080 du container sur le port 80 de l'hôte
* __-t__ : alloue un terminal (pseudo-TTY)
* __-i__ : mode interactif, conserve le flux d'entrées ouvert (STDIN)
* __node__ : l'image a partir de laquelle créer le container
* __/bin/bash__ : la commande a executer

Vous vous trouvez actuellement dans un container disposant de Node.js.
{% highlight bash %}
node --version && npm --version
{% endhighlight %}

Nous pouvons lancer le serveur :
{% highlight bash %}
node server.js
{% endhighlight %}
