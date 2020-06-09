# applicationchat-certificat
# applicationchat-certificat 
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
        je vais m attarder un peu dans l exlication des commandes donc on a utilise directement l outil keytool disponible de java on peut aussi utiliser l outil PKCS12 et le convertir en keystore mais sa aura le meme resultat 
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
