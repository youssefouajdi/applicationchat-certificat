
## applicationchat-certificat 
# le projet contient 3 etapes :
  - signature electronique
  - certificat electronique
  - protocole electronique 
    RESUME l ensemble de toute les etapes : certificat electronique + RSA cote server + cryptage symetrique cote client 
 
# 1- la signature l electronique 
     comme la signature normal pour retirer des documents administratif elles permet de reconnaitre la personne .
        la signature electronique permet de reconnaitre  aussi la personne la signature electronique est disponible on ouvrant un fichier           pdf et puis cliquer sur signer le  document
        
# 2- certificat electronique 
        assurez vous d aboard de la version que vous diposez tapez la commande sur cmd : java -version 
        vous diposez de 2 methodes soit creer un certificat vous meme ou bien telecharger chilkatsoft disponible pour les versions 11 (j ai pas tester pour les autres versions je n ai pas trouver ) je met a votre disposition les liens pour utiliser le chilkatsoft qui se base sur  Load Base64-encoded X.509 Certificate (.cer)
        llien de telechargement :http://www.chilkatsoft.com/java.asp
        lien de code : https://www.example-code.com/java/cert_load_base64.asp
        si vous avez choisi de creer vous meme votre certificat suivez ce qui suit 
        <img src=“C:\Users\youssef\Desktop\cap.png”>
![cap](https://user-images.githubusercontent.com/47403132/84211363-79f61c80-aabb-11ea-81ef-a61bdfb432b8.png)
        vous disposez finaleement d un fichier .cer
        je vais m attarder un peu dans l exlication des commandes donc on a utilise directement l outil keytool disponible de java 
        la premiere commande on lui demande de generer un keystore avec comme alias ou identifiant youssef on utilisant le cryptage asymetrique RSA ( ou bien le cryptage que vous allez utiliser vous cote server ) et puis l emplace ou le ficher va etre creer : son_nom.jks
        il va vous demander aussi le password le nom que vous allez retrouvez apres sur les proprietes de votre certificat la location organisation etc ... il vous donne comme resultat un RSA 2048  bit et de 90 jours de validite par defaut notez bien que vous pouvez la modifier par ligne de commande 
        puis pour generer le certificat vous allez utilisez la 2 eme commande en mode export vous allez preciser 
        __le meme alias que vous avez utilisez avec la premiere commande sinon il ne la reconnait pas__        
        le fichier ou vous voulez  mettre votre fichier et quel jks vous voulez utilisez (le jks que vous venez de cree)
        on peut aussi utiliser l outil PKCS12 et le convertir en keystore mais sa aura le meme resultat 
        Pour les gens qui veulent utiliser PKCS12 suivez les etapes suivantes mais avant on doit comprendre ce que keystore
        Apache Tomcat et de nombreuses autres applications Java prévoient de récupérer des certificats SSL / TLS à partir de clés Java           (JKS). Les machines virtuelles Jave sont généralement fournies avec keytool pour vous aider à créer un nouveau keystore.

Keytool vous aide à:

- créer un nouveau JKS avec une nouvelle clé privée(c est le but soit de la premiere methode direct ou bien par pkcs12)
- générer une demande de signature de certificat (CSR) pour la clé privée dans ce JKS
- importer un certificat que vous avez reçu pour cette CSR dans votre JKS
Keytool __ne vous permet pas d'importer une clé privée existante pour laquelle vous avez déjà un certificat__.
Vous devez donc le faire vous-même, voici comment:
 - regardez la video suivante https://www.youtube.com/watch?v=1xtBkukWiek
 - Supposons que vous ayez une clé privée (key.pem) et un certificat (cert.pem), tous deux au format PEM comme le suggèrent les noms de fichiers.
 Le format PEM est  lisible par l'homme  et ressemble par exemple à
 
 *-----BEGIN CERTIFICATE-----
Ulv6GtdFbjzLeqlkelqwewlq822OrEPdH+zxKUkKGX/eN
.
. (snip)
.
9801asds3BCfu52dm7JHzPAOqWKaEwIgymlk=
----END CERTIFICATE-----*
Convertissez les deux, la clé et le certificat au format DER en utilisant openssl
les commande sont du format suivant :
 - openssl pkcs8 -topk8 -nocrypt -in key.pem -inform PEM -out key.der -outform DER
 - openssl x509 -in cert.pem -inform PEM -out cert.der -outform DER
 Vient maintenant la partie délicate, vous avez besoin de quelque chose pour importer ces fichiers dans le JKS. ImportKey le fera pour vous, obtenez la source ImportKey.java (text / x-java-source, 6,6 kB, info) ou la clé ImportKey.class compilée (Java 1.5!) et executez 
 *user@host:~$ java ImportKey key.der cert.der
Using keystore-file : /home/user/keystore.ImportKey
One certificate, no chain.
Key and certificate stored.
Alias:importkey  Password:importkey*
sinon pour la deuxieme methode la plus simple vous allez suivre l image precedante vous allez obtenir le meme resultat
Voila vous disposez maintenant d un certificat electroniques maintenant reste a voir comme l exporter a un client dans notre application de chat / l importer dans notre code .
Avant de finir je vous recommande fortement d utiliser la methode pem plus difficil mais elle permet de voir la cle public la cle privee et de creer a partir de pkcs12 un keystore
# facultatif
Avant de commencer la derniere etape on a etudier le hmac j ai voulu faire l exmple pour savoir sa puissance 
En cryptographie, un HMAC est un type spécifique de code d'authentification de message impliquant une fonction de hachage cryptographique et une clé cryptographique secrète
Cet exemple de code montre comment calculer un Hmac-Sha256 à partir d'un message et d'une clé secrète (à l'aide de java 12 ou version ultérieure).
![CaptureHMAC](https://user-images.githubusercontent.com/47403132/84699994-c4134e00-af52-11ea-97cd-c2064f89c235.PNG)
Nous pouvons tester la méthode calcHmacSha256 à partir d'une classe principale qui imprime le Hmac à console en tant que chaîne hexadécimale.
![CaptureMain](https://user-images.githubusercontent.com/47403132/84700291-2ff5b680-af53-11ea-88cc-54ccc496a23d.PNG)

Le Hmac-Sha256 pour ce message particulier et cette clé secrète est:
*Hex: 57938295649097379cddb382dd6c82d5e0460645a8fd01674a48a76de6142646*


Alors qu'une représentation sous forme de chaîne hexadécimale convient pour le transport d'un Hmac dans un appel de service ou une liaison, une représentation plus efficace est base64. L'extrait de code suivant utilise java.util.Base64.
__String base64HmacSha256 = Base64.getEncoder().encodeToString(hmacSha256);
  System.out.println("Base64: " + base64HmacSha256);
  
Comme vous pouvez le voir, la chaîne base64 est plus courte.
*Base64: V5OClWSQlzec3bOC3WyC1eBGBkWo/QFnSkinbeYUJkY=*

# protocole electronique 
je n ai pas trouver comment passer de certificat x509 a pkcs8 directement mais on peut convertir certificat en pem
openssl x509 -in certificatename.cer -outform PEM -out certificatename.pem
puis pem en pkcs8
openSSL pkcs8 -in certificatename.pem -topk8 -nocrypt -out certificatename.pk8
sinon une solution meilleur consiste a generer une cle RSA
![Capture6](https://user-images.githubusercontent.com/47403132/84516734-7edeea00-acce-11ea-8546-318aa598af4d.PNG)
L'exemple ci−dessus génère une clé RSA de 1024 bits.
Il faut ensuite convertir la clé RSA au format DER avec PKCS#8.
![Capture7](https://user-images.githubusercontent.com/47403132/84516867-b3eb3c80-acce-11ea-94a9-a9e48de5b57b.PNG)
finalement on peut utiliser le fichier pkcs8 obtenu pour code le message 
![Capture+java](https://user-images.githubusercontent.com/47403132/84698570-5108d800-af50-11ea-9886-968b94584963.PNG)
le code cryptage
![Capture8](https://user-images.githubusercontent.com/47403132/84516875-b64d9680-acce-11ea-8d28-2041d4f1cb69.PNG)


maitenant on doit integrer le certificat cote server/ client on va proceder de la maniere suivante 
cote client
![Captureclient](https://user-images.githubusercontent.com/47403132/84697654-d25f6b00-af4e-11ea-8488-f5006466ebdb.PNG)
info cote client 
![Captureclient](https://user-images.githubusercontent.com/47403132/84697654-d25f6b00-af4e-11ea-8488-f5006466ebdb.PNG)
cote server 
![Captureserver](https://user-images.githubusercontent.com/47403132/84698040-6e897200-af4f-11ea-9aa6-39679588fa9b.PNG)


