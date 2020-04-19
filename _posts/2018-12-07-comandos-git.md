---
layout: post
title: "Comandos git"
date: 2018-12-07 11:32:00 -0500
categories: development
tags: [development, github, git, macos, terminal, https, ssh, version 
control]
---
Bueno, aqui estoy de vuelta... Con un poco de desarrollo para amenizar 
el dia... Asi que les cuento...

En octubre hubo un evento llamado Hacktoberfest donde impulsaban el uso 
de Pull Request en Github... Que es lo uno? Que es lo otro? Empecemos 
por explicar git. 

## Git ##

Git es una herramienta en linea de comandos que nos ayuda a llevar 
control sobre los cambios realizados sobre un proyecto/archivo/programa/lo 
que sea. Por ejemplo, voy a iniciar un documento para una tesis (estos 
documentos tienden a ser grandes, de muchos capitulos, muchos cambios), 
entonces lo que hago es inicializar un repositorio con el comando 
<code>git init</code>. Con esto le hemos dicho a git que lleve un control 
sobre la carpeta donde ejecutamos el comando. Luego debemos decirle cual 
archivo debe controlar, el cual debe estar en la misma carpeta, 
<code>git add &lt;nombre de archivo&gt;</code> y para confirmar el control 
hacemos lo que se denomina un commit <code>git commit -m 
&quot;&lt;descripcion&gt;&quot;</code>. Con estos tres comandos ya estamos 
llevando control sobre un archivo. Si se desea llevar control sobre mas 
archivos, hacemos git add sobre esos archivos o podemos cambiar el 
nombre del archivo por un * para decir que queremos llevar control sobre 
todos los archivos en esa carpeta.

Muy bien, con eso estamos listos. Despues de un tiempo, se hacen cambios 
en el archivo, guardamos. Para ver que git detecta los cambios 
realizados podemos ejecutar <code>git status</code> y nos 
responder&aacute; que el archivo tiene cambios. Para registrar esos 
cambios usamos <code>git commit -am &quot;&lt;descripcion&gt;&quot;</code>. Una vez 
hecho esto, podemos revisar una vez mas y git nos dira que esta 
actualizado, no hay que hacer nada. Hasta ahi todo bien, podemos llevar 
control de cambios sobre nuestro archivo. Cada vez que hagamos algo sobre el archivo, ejecutar este ultimo comando para llevar el control.

Supongamos el caso que alguien abre el archivo y nos borra todo! Y 
encima guarda el archivo! PERDIMOS TODO!!! PANICO, CORRAN! PANICO, 
CORRAN!

MOMENTITO! git nos puede ayudar a resolver el problema. Si ejecutamos 
<code>git status</code> nos dir&aacute; que hay cambios sin registrar. 
Por lo que podemos ejecutar <code>git checkout -- &lt;nombre del 
archivo&gt;</code> y voil&aacute;! Nuestro archivo regresa a ser el de 
antes. PHEW!

## Github ##

Github es un sitio en internet que permite almacenar de manera remota 
repositorios que usen <code>git</code>. Hace poco fue comprado por 
Microsoft. Tambien existen otros sitios que ofrecen este servicio de 
almacenamiento, Bitbucket por ejemplo. Este tipos de sitios son muy 
usados por desarrolladores para aportar su granito de arena en el 
desarrollo global.

Siendo asi, ya teniendo una [cuenta 
creada](https://help.github.com/articles/signing-up-for-a-new-github-account/), 
[crearemos un repositorio](https://help.github.com/articles/create-a-repo/) 
(una carpeta) donde guardaremos el archivo. Una vez creado el repositorio, debemos enlazar 
nuestra carpeta en la computadora con la de Github. Para esto usamos el comando 
<code>git remote add origin https:&#47;&#47;github&#46;com/&lt;nombre de 
usuario&gt;/&lt;nombre de repositorio&gt;</code>, confirmamos que se 
haya hecho el cambio con el comando <code>git remote -v</code>, nos 
deben aparecer dos direcciones web. 
[[Fuente]](https://help.github.com/articles/adding-a-remote/)

Una vez hecho esto, podemos publicar en la web mediante el comando 
<code>git push origin master</code>. Cuando esto termine, podremos ir a 
http:&#47;&#47;github&#46;com/&lt;usuario&gt;/&lt;repositorio&gt; y veremos nuestro 
codigo publicado. 
[[Fuente]](https://help.github.com/articles/pushing-to-a-remote/)

## Pull Request ##

Todos conmigo hasta ahora? Muy bien! Que es un Pull Request? Como les 
habia dicho antes, Github es un sitio en la web donde se hospeda codigo. 
Lo que aun no les digo es que ese codigo puede ser copiado y trabajado 
por multiples personas de manera colaborativa. Siendo asi, yo puedo 
[copiar un repositorio](https://help.github.com/articles/about-forks/) 
de alguien mas, modificar el codigo, hacer ajustes, etc. Si estos cambios 
son considerables, son para mejorar el bien comun, puedo solicitarle 
al due&ntilde;o del repositorio que lo asimile al codigo original, 
esto es un Pull Request. 
[[Fuente]](https://help.github.com/articles/about-pull-requests/)

## Hacktoberfest ##

Hacktoberfest es un evento en celebracion al software de codigo abierto, 
dirigido por la empresa Digital Ocean, en conjunto con Github y Twilio. Es un 
evento abierto para todo el mundo. Este evento dura todo el mes de octubre, 
por lo cual se puede ingresar en cualquier fecha dentro del mes. Para 
participar, se debe realizar 5 Pull Requests validos. Al finalizar el evento, 
si los Pull Requests son validos, te regalan una camiseta con stickers! Yo 
obtuve la mia por primera vez este 2018 y pienso volverlo a hacer el 
a&ntilde;o que viene.

Honestamente, basto solo eso para reintegrarme con una gran comunidad de 
programadores y fortalecer ciertos conceptos que no habia practicado en
mucho tiempo, especialmente con git. Y para no olvidarlos, lo 
publiqu&eacute; aqui, esperando que a alguien mas le sea de provecho.

Hasta la siguiente!
