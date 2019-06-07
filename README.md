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
