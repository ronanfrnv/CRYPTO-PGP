# CRYPTO-PGP | GPG

Sur linux `apt-get install gnupg`

PGP
  - Chiffrement hypride (Symétrique et Asymétrique)
  - Algo symétrique n'est pas en licence libre

Gnupg
  - Chiffrement hybride
  - Fonctionnement légèrement différents

Le fichier gpg.conf est essentiel pour fixer certain paramètre de GPG.

VeraCrypt est un logiciel utilitaire sous licence libre utilisé pour le chiffrement à la volée (OTFE). Il est développé par la société française IDRIX et permet de créer un disque virtuel chiffré dans un fichier ou une partition. L'ensemble du dispositif de stockage demande une authentification avant de monter le disque virtuel. 

/root/.gnupg/gpg.conf si vous êtes admin 
/home/user/.gnupg/gpg.conf si vous êtes sur le compte user. 

IL y a plusieurs algorithmes de clés symétriques utilisables par GPG `gpg --version`
Chiffrement : IDEA, 3DES, CAST5, BLOWFISH, AES, AES192, AES256,
              TWOFISH, CAMELLIA128, CAMELLIA192, CAMELLIA256
              
Commande pour générer la clé `gpg --full-generate-key `

Pour regarder les clés : `gpg –k` (clé publique) et `gpg –K` (clés secrètes) 

SC = clé primaire
E = Encrypt
S = Signature

Clé publique (public key) :
        Type : RSA 2048 bits
        Identifiant : EC4EE226BF28FA70ACD6295491AA3D54E75F528C
        Utilisation (usage) : [SC] (Signature et Chiffrement)
        Description : La clé publique est utilisée pour chiffrer des messages à votre intention et vérifier les signatures que vous avez créées. Les autres personnes peuvent utiliser cette clé publique pour communiquer de manière sécurisée avec vous et vérifier votre identité en vérifiant les signatures que vous avez apposées sur des documents.

    Sous-clé (subkey) :
        Type : RSA 2048 bits
        Utilisation (usage) : [E] (Chiffrement)
        Description : La sous-clé est une clé de chiffrement supplémentaire associée à votre clé principale. Elle peut être utilisée pour le chiffrement de messages, ce qui permet de séparer les fonctions de chiffrement et de signature dans des clés distinctes. Cela offre une plus grande flexibilité en termes de gestion des clés et de sécurité.

Il est important de noter que la clé privée correspondante à la clé publique est nécessaire pour déchiffrer les messages chiffrés à l'aide de votre clé publique et pour signer des documents avec votre signature. Assurez-vous de conserver votre clé privée en lieu sûr et de la protéger avec une passphrase forte.

Les clés publiques peuvent être partagées avec d'autres utilisateurs afin qu'ils puissent vous envoyer des messages chiffrés ou vérifier les signatures que vous avez créées. Les clés privées, en revanche, doivent rester confidentielles et ne doivent pas être partagées ou exposées.

La commande `gpg --edit-key` permet de créer une sous clé pour signer les documents
Puis add Key -> puis le type de la clé choisi

Pourquoi faut-il générer tout de suite le certficiat de révocation ? `gpg --output revoke_cert.asc  --gen-revoke <ID_CLÉ_PRIVEE>`

Un certificat de révocation est un document signé qui indique que votre clé privée n'est plus valide et ne doit plus être utilisée. Il s'agit d'un mécanisme de sécurité important en cas de perte, de compromission ou de non-utilisation future de votre clé privée. Le certificat de révocation permet d'informer les autres utilisateurs que votre clé n'est plus fiable et qu'ils ne doivent pas l'utiliser pour chiffrer des messages ou vérifier des signatures.

Pour afficher les listes des clés

`gpg --list-keys` | `gpg --list-secret-keys`

Pour afficher la liste des signatures  `gpg --list-sigs`

Pour exporter `gpg --export-secret-keys --armor > /home/kali/secret_key.asc`
Pour exporter sa  sous clé `gpg --export-secret-subkeys --armor --output chemin_vers_fichier ID_sous_clé`

Pour exporter sous format ASCII

`gpg --export-secret-keys --output /root/cle_GPG/secret_key.gpg`

Pour exporter sous format BINAIRE
`
sudo gpg --export-secret-keys --armor > /root/cle_GPG/secret_key.asc
sudo gpg --export-secret-keys --output /root/cle_GPG/secret_key.gpg
`

Pour supprimer les clés publiques (Il faut au préalable supprimer la clé privé)

`gpg --delete-keys --batch --yes <ID_de_la_clé>`

Pour supprimer les clés privés 

`gpg --delete-secret-keys --batch --yes <ID_de_la_clé>`

Pour importer les clés `gpg --import subkey1.asc`

Obtenir le fingerprint `gpg --fingerprint NOMDELACLE`

L'empreinte de la clé, également appelée empreinte digitale ou fingerprint, est une représentation unique et compacte de la clé publique. Elle est généralement affichée sous la forme d'une séquence de chiffres hexadécimaux. L'empreinte permet d'identifier de manière univoque une clé spécifique parmi un grand nombre de clés.

Pour signer une clé il faut importer la clé puis utiliser la commande `gpg --edit-key "lenomdelaclé" `
Puis entrer la commande `trust`

Pour exporter une clé contre signé
`gpg --armor --export CLE > test.asc`




