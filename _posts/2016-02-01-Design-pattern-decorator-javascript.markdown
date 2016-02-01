---
layout: post
title:  "Implémentation du design pattern decorator avec Node.js"
date:   2016-02-01 19:28:00
categories: nodejs
banner: nodejs.png
---
Le patron de conception décorateur permet d'ajouter dynamiquement des fonctionalités à un objet. Afin de l'implémenter en JavaScript nous allons avoir besoin d'un prototype Component qui permet de définir qu'un objet peut être décorer. Toutes nos instances décorées hériterons de ce prototype.

Créons donc le fichier component.js
{% highlight javascript %}
module.exports = component

function component()
{

}
{% endhighlight %}

Nous allons ensuite créer un prototype qui serra capable d'ajouter des fonctionalités à un component, il s'agit du prototype decorator dont tous nos decorateur hériterons.

Créons donc le fichier decorator.js
{% highlight javascript %}
var Component = require('./component')

module.exports = decorator

function decorator(component)
{
    if ( !(component instanceof Component)) {
        throw 'Invalid type of component'
    }
    this.component = component
}
{% endhighlight %}
Un décorateur prend donc en paramètre un composant.

Nous pouvons maintenant créer un décorateur dans le fichier log.js.
{% highlight javascript %}
var Decorator = require('./decorator')
var util = require("util")

module.exports = log

function log(component) {
    log.super_.call(this, component);
    this.component.log = function(msg) {
        console.log(msg)
    }
}
util.inherits(log, Decorator)
{% endhighlight %}

Puis notre composant dans le fichier dummy.js
{% highlight javascript %}
var Component = require('./component')
var util = require("util");

module.exports = dummy

function dummy() {
    thing.super_.call(this);
}
util.inherits(thing, Component);
{% endhighlight %}

Il ne reste plus qu'a instancier notre prototype dans index.js
{% highlight javascript %}
var Dummy = require('./dummy')
var Log = require('./log')

var d = new Dummy()
new Log(d)
d.log('hello world !')
{% endhighlight %}
