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
mkdir node_serveur_project && touch $\_/server.js
{% endhighlight %}

Le fichier node_serveur_project/server.js doit contenir un serveur tel que :
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

Rendons nous dans le dossier node_serveur_project et connectons nous à un container grace à la commande suivante :

{% highlight bash %}
docker run --rm -ti -w /srv -p 8080:8080 -v $PWD:/srv node /bin/bash
{% endhighlight %}

Vous vous trouvez actuellement dans un container disposant de Node.js.
{% highlight bash %}
node --version && npm --version
{% endhighlight %}

Nous pouvons lancer le serveur :
{% highlight bash %}
node server.js
{% endhighlight %}
