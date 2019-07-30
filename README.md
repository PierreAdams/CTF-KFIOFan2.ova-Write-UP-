CTF disponible à cette adresse : https://khaos-drive.mycozy.cloud/public?sharecode=MDguO1eH68Ma

scan nmap habituel : 

![Scan](https://user-images.githubusercontent.com/39098396/59051726-df527480-888d-11e9-96f8-8e31c705948c.png)

Aucun port ouvert donc on elargit le scan de ports : 

![Scan](https://user-images.githubusercontent.com/39098396/59051726-df527480-888d-11e9-96f8-8e31c705948c.png)

pour pousser plus loin le scan on peut meme faire un 

```# nmap -sV -sC 192.168.1.23 -p 26921``` 

qui nous revele que l'utulisateur Anonymous peut se connecter 

Le port 26921 est ouvert (vsftpd 2.0.8) 

On va ecouter sur le port 26921 avec Netcat : 

![Scan](https://user-images.githubusercontent.com/39098396/59052385-4ae91180-888f-11e9-8e26-bbd2e8582294.png)

Le message qui apparait : 220 Salut Alice ! Suite a l'attaque sur notre precedent serveur, j'en prepare un nouveau qui sera bien plus securise ! C'est en travaux pour l'instant donc s'il te plait ne touche a rien pour l'instant... Bob


Pour la suite On va se connecter en FTP Avec un Outil comme FilleZilla (celui que j'utilise sur mon kali mais il en existe d'autres) En Anonymous (On à vu que c'etait possible)

![Scan](https://user-images.githubusercontent.com/39098396/59053458-c8ae1c80-8891-11e9-9d44-64f0ee84f0d3.png)

On a donc accès à 4 images et a un dossier Serrure ! On les telecharge pour les analyser et voir ce qu'on peut en tirer!

![Scan](https://user-images.githubusercontent.com/39098396/59053607-16c32000-8892-11e9-99f8-1d10ab9c49c6.png)

si on rassemble les images pour reconstituer le corps de Chaos nous pouvons dechiffré au milieu : 

```cle.txt```

cette même clé qu'on va mettre dans la serrure et voir ce qui se passe : 

![Scan](https://user-images.githubusercontent.com/39098396/59113195-e5575c80-8944-11e9-96e4-1c898728821c.png)

Sesame...

![Scan](https://user-images.githubusercontent.com/39098396/59113895-43387400-8946-11e9-95b2-f3349fac0d4f.png)

nous avons donc un acces http sur le port 26980

avec un misterieux message : 

![Scan](https://user-images.githubusercontent.com/39098396/59130093-c5d52980-896e-11e9-8caa-ec5ff44e173b.png)

Avec comme code source : 

![Scan](https://user-images.githubusercontent.com/39098396/59130296-5ad82280-896f-11e9-953e-79c4abd1b7af.png)

Si on se renseigne un peu sur l'auteur et sur l'origine de son nom on peut tomber sur son propre blog : 

![Scan](https://user-images.githubusercontent.com/39098396/59135092-fc667080-897d-11e9-8332-d137d40ddb2b.png)

Si on met le nom complet de Khaos (c'est a dire : Khaos _Farbauti Ibn Oblivion_ ) dans le fichier cle.txt

![Scan](https://user-images.githubusercontent.com/39098396/59135266-9af2d180-897e-11e9-8771-6422ec91c063.png)


Un Formulaire d'upload est soudain apparu :


![Scan](https://user-images.githubusercontent.com/39098396/59135424-22404500-897f-11e9-84dc-67ba6fa0384b.png)

Apres quelque recherches nous allons devoir uploader 2 fichiers :
  - Un .htaccess : 
  
  
  ![Scan](https://user-images.githubusercontent.com/39098396/59339404-6a0af780-8d04-11e9-90da-31c11ba3bdaf.png)
  
  - Un Command-Shell : 
  
  
   ![Scan](https://user-images.githubusercontent.com/39098396/59339558-ae969300-8d04-11e9-8dc7-bb87837fbca5.png)
   
 Une fois ces 2 fichiers uploader 
 on se rend dans les fichier uploader pour voir notre shell : 
 
 _http://192.168.1.23:26980/uploads/_
 
 et on ouvre notre shell pour pouvoir executer des commandes comme ceci : 
 
 
 ![Scan](https://user-images.githubusercontent.com/39098396/59340256-ece08200-8d05-11e9-8b04-d6c0c6c022e7.png)


on peut ensuite voir une cle rsa ici : 


 ![Scan](https://user-images.githubusercontent.com/39098396/59344231-f1109d80-8d0d-11e9-9afc-a5a78f8d4b39.png)
  
  
En meme temps, un nouveau port c'est ouvert : 


![Scan](https://user-images.githubusercontent.com/39098396/59344499-801db580-8d0e-11e9-9173-c87ea992fb1c.png)

Avec un scan plus poussé nous voyons que c'est un serveur SSH : 

![Scan](https://user-images.githubusercontent.com/39098396/59344697-e4d91000-8d0e-11e9-81be-b2cf088928f9.png)

Avec notre clé RSA nous tentons de nous connecter sur ce serveur avec les utilsateur connu (Alice ou BOB)


_ssh -p 26922 bob@192.168.1.23 -i rsa_

Une fois un acces SSH, nous pouvons voir aisément que nous avons affaire à un buffer overflow 

Nous commencons par faire un 

_strings test_ 

Et parmis les reponses de cette commande nous voyons un : 

![Scan](https://user-images.githubusercontent.com/39098396/62124099-a94bd280-b2c9-11e9-9601-55e73c8f0e6b.png)

*lancement debug*
*touch /root/authorize_bob*

il nous faut donc activer le debug pour que logiquement bob soit authoriser en tant que root sur la machine.

pour ca allons voir l'adresse de la memoire de debug : 

![Scan](https://user-images.githubusercontent.com/39098396/62121038-1ad45280-b2c3-11e9-8d71-437cc52dfc02.png)

une fois cette adresse memoire copier on va voir à partir de combien de caractere le debordement de tampon ce fait.

![Scan](https://user-images.githubusercontent.com/39098396/62121542-18262d00-b2c4-11e9-8a66-5e868608cd29.png)


![Scan](https://user-images.githubusercontent.com/39098396/62122510-270ddf00-b2c6-11e9-96a6-c54ab458593d.png)

le debordement ce fait donc à 21 caracteres.

Nous allons maintenant reprendre notre addresse de Debug afin de l'ajouter à la suite de notre chaine de caractere 
mais en le convertissant en little Endian ce qui fait : 
\x20\x48\x55\x55\x55\x55

nous allons donc envoyer ceci : 

_python -c 'print ("123456789abcdefghijkl"+"\x20\x48\x55\x55\x55\x55")' | ./test_

nous voyons bien le lancement du debug 

nous pouvons ensuite faire un sudo su 
et nous somme root ! 

![Scan](https://user-images.githubusercontent.com/39098396/62123614-9b498200-b2c8-11e9-98c8-4d3719e493c6.png)




