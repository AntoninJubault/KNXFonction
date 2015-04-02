# KNXFonction
L'objectif de cette classe est d'aider les communication avec votre matériel KNX.
PS : N'oubliez pas d'importer la classe Caliméro.

Tout d'abord merci à Johann Bourcier pour le cours dispensé sur KNX.

Pour cela l'intégralité de la gestion de votre système KNX ne SERA PAS réalisé sous ETS.

Vous devrez au prélable créer une architecture sous KNX vous permettant de bien identifier vos participant. 

Pour illustrer l'utilisation de cette classe, je vais vous expliquer dans quel contexte je l'ai créée:

OBJECTIF : Utiliser une application Android qui va executer des actions sur le réseau KNX.
Celle-ci communique en REST avec le serveur situé sur mon PC. Mon PC reçoit les information
puis communique via la bibliothèque Caliméro avec la maquette KNX.

Dans cet exemple nous allons chercher à récupérer l'état d'une lampe et choisir de la mettre à 1 ou non.
Il y aura 3 lampes.

Configuration du la topologie du résaau KNX et de ses participant.

    Commencez par importer via la bibliothèque chacun des participants.
    Nous allons ensuite créer des adresses de groupes afin de correctement différencier les fonctions liées aux participants.
    J'ai donc créé une topologie en trois étages :
    
    Le premier étage indique que je m'intéresse aux Lampe (0), le deuxièmeà leurs état, et le troisième au numéro du participant
    (numéro de la lampe)
    Nous obtenons donc pour récupérer l'état de la lampe 1 :  0 (Lampe) / 0(Etat) / 1 (Lampe 1)
    
    Idem pour actionner les lampes : 0(Lampe) / 1 (Ecrire) / 1 (Lampe 1)
    
    Nous aurons donc ainsi sous ETS avec par exemple un actionneur RMG 4I
    0/0/1 : retour d'état du RMG 4I actionnant la lampe n°1
    0/0/2 : retour d'état du RMG 4I actionnant la lampe n°2
    0/0/3 : retour d'état du RMG 4I actionnant la lampe n°3
    
    0/1/0 : ATTENTION ==> laisser cette partie nulle, sinon vous décalerez tout, il faudra actionner la lampe 2 pour actionner la lampe 1 etc.. 
    0/1/1 : retour d'état du RMG 4I actionnant la lampe n°1
    0/1/2 : retour d'état du RMG 4I actionnant la lampe n°2
    0/1/3 : retour d'état du RMG 4I actionnant la lampe n°3

    A vous de créer votre stucture de données comme vous l'entendez.
    
    Utilisation de la classe :
    Mon application envoi par exemple une requete REST de type GET sur http://AdresseDeMonServeurPc:8080/rest/AllumerLampe1
    
    Sur mon serveur on retrouvera 
    @GET
    @Path("/AllumerLampe1")
    public void TurnOnLampe1(){
      KNXFonction.Allumer(0,1,1);
    }
    
    Avec cette classe vous pourrez : changer l'état d'un participant, écrire 1 ou 0 dans un participant, lire l'état d'un participant
    
    
    Ce n'est qu'un exemple, à vous de transformer tout cela en requetes POST et personnaliser votre application.
    
    Toute la force de ce principe est que, si ce que vous fournis par exemple hager comme design ne vous convient pas où est trop compliqué,
    Vous pouvez développer votre propre application.
    
