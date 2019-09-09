# projet-d-tection-d-angle-reconstruction
La problématique de ce projet est de reconstituer une image représentant un paysage incliné en la même image avec le même paysage non incliné. De plus, on cherche à effectuer ce travail sans tronquer l'image initiale.Lors de la prise de photo, particuliérement dans le cadre de l'utilisation du zoom, une légére rotation non désirée de l'appareil peut aboutir à un résultat plus ou moins incliné. Cela peut aboutir au rejet de la photo car la symétrie de ce qui est représenté est brisée et l'esthétique altérée.Bien entendu, des applications existent pour recadrer la photo. Cependant celles-ci doivent effectuer une combinaison de deux procéssus : une rotation et un zoom.La phase de zoom, s'accompagne donc nécessairement d'un découpage de l'image et donc d'une perte d'information.L'objectif de ce projet est double : premièrement détecter automatiquement l'angle d'inclinaison de l'image. Cet angle varie entre -pi/2 et +pi/2 radians. Cela constitue un exercice non trivial puisque l'algorithme doit comprendre les objets inhérents à l'image. Il doit par exemple identifier des arbres sur une image de forêt et comprendre que ces derniers poussent à la verticale, il doit aussi pouvoir comprendre la topographie de ce que représente l'image et pouvoir identifier des montagnes (la zone de démarcation entre le ciel et la terre n'est donc pas forcément horizontale).Pour effectuer ce travail nous utilisons un réseau de neurones pré-entrainés sur imageNet, et réalisons un exercice assez analogue à celui que nous avions réalisé dans le cadre de l'identification de races de chiens. Nous utilisons un réseau de type ResNet50 car ce dernier est de dimmension relativement modérée en comparaison d'un Xception. Nous assumons que le gain de performance de réseaux plus profonds ou contenant plus de paramètres n'est pas utile puisque nous cherchons à identifier des structures assez générales (ciel, nuages, arbres, collines). Nous assumons par ailleurs que le corpus d'entrainement (ImageNet) contient suffisamment d'images de paysages pour identifier les structures caractéristiques. Nous ajoutons deux couches denses de 500 et respectivement 50 neurones, un dropout de 0.25 et une batch-normalisation, nous utilisons en sortie un seul neurone dont le rôle est de donner l'angle de rotation. Nous ramenons les angles à une valeur entre 0 et 1 et utilisons une fonction d'activation de type sigmoïde. Nous utilisons une erreur de type RMSE. Nous partageons dés le début notre dataset d'images appareillées générées en trois groupes (entrainement, validation et test) et calibrons l'arrêt de l'entrainement en visualisant la fonction de coût sur le jeu de validation. Nous analysons les résultats sur le jeu de test. Après 150 epochs et en utilisant une taille de mini-batch de 10 nous obtenons une erreur de quelques degrés, bien inferieure à l'erreur statistique moyenne d'environ 25 degrés. Notons que le corpus d'entrainement contient des images de paysages variés dont l'angle d'inclinaison peut parfois être difficilement interprétable à l'oeil nu. Nous pourrions envisager d'obtenir de meilleurs résultats en réentrainant les dernières couches du resnet50 en partant du principe que les structures qui interviennent dans le calcul de l'angle ne sont pas forcément bien représentées par la sortie du resnet50. Il est tout à fait possible de générer des données d'entrainement. Il suffit, de prendre des images dont le contenu représenté est jugé droit, d'effectuer une combinaison rotation/zoom de sorte à obtenir une nouvelle image dont le contenu est incliné de l'angle désiré. Nous effectuons ce travail en faisant varier l'angle d'inclinaison de -pi/5 à +pi/5 radians de manière complètement aléatoire. Notre dataset initial contient 3600 images et nous effectuons 3 itérations ce qui équivaut à effectuer de la data-augmentation.
