# Scénarios de validation

Ce projet étudie trois situations de sécurité Wi-Fi dans un laboratoire Kali Linux. Le module `mac80211_hwsim` fournit quatre radios virtuelles : un point d’accès légitime, une station de test, une interface de capture et une interface utilisée pour le point d’accès jumeau selon le scénario. Cet environnement permet de travailler sur les échanges d’authentification sans équipement radio physique. Les observations présentées concernent uniquement ce laboratoire.

## 1. WPA2-PSK : capture et vérification par dictionnaire

Le premier scénario utilise un point d’accès configuré avec `hostapd` en WPA2-PSK et AES-CCMP. Une station contrôlée, gérée par `wpa_supplicant`, établit une connexion. Une interface en mode moniteur permet ensuite d’observer les échanges EAPOL liés à la négociation des clés.

Le résultat attendu est une capture contenant les éléments nécessaires à la vérification de candidats, puis leur conversion dans un format accepté par Hashcat. Le rapport documente l’extraction d’une paire EAPOL et la récupération du secret de test par dictionnaire. Ce secret, volontairement faible, faisait partie des candidats disponibles.

Cette manipulation illustre la faiblesse d’une phrase secrète prévisible. Elle ne démontre pas une rupture du chiffrement AES : le traitement consiste à vérifier des hypothèses de mot de passe à partir de l’échange capturé. La tentative de désauthentification affiche par ailleurs zéro acquittement ; ce seul indicateur ne permet pas d’attribuer avec certitude la reconnexion à l’injection.

## 2. WPA3-SAE : un résultat limité par la capture

Le deuxième scénario prévoit une configuration WPA3-Personal utilisant SAE, avec les trames de gestion protégées obligatoires (`ieee80211w=2`). L’objectif était de comparer cette configuration au scénario WPA2 en réutilisant la chaîne de conversion et de traitement.

Le traitement n’a produit aucun hash chargeable. L’examen des traces apporte cependant une limite importante : une capture contient très peu de paquets et signale l’absence de trames EAPOL et d’authentification utiles. De plus, une illustration présentée comme WPA3 reprend une vue WPA2 ; elle ne peut pas servir de preuve indépendante.

Le résultat retenu est donc l’échec du traitement de la capture fournie. Il ne suffit pas à établir une comparaison expérimentale concluante entre WPA2 et WPA3. Une nouvelle acquisition, dont le protocole et la complétude seraient vérifiés, serait nécessaire pour approfondir cette partie.

## 3. Evil Twin : association contrôlée au point d’accès jumeau

Le troisième scénario met en place deux points d’accès ayant le même SSID et des BSSID différents. Le jumeau est configuré avec le secret de test déjà connu. La station est dirigée vers lui par un paramètre `bssid=` explicite dans son profil.

L’observation attendue est une association au BSSID du jumeau et la capture de son échange EAPOL. Le rapport documente cette association, l’extraction d’une paire exploitable et une nouvelle récupération par dictionnaire.

L’expérience montre une duplication du nom du réseau et une association volontaire au clone. Elle ne démontre ni un basculement spontané vers le signal le plus puissant, ni un détournement complet du trafic, ni une compromission initiale sans connaissance du secret. Ces distinctions limitent les conclusions au scénario effectivement réalisé.

## Références techniques

- [Linux Wireless : mac80211_hwsim](https://wireless.docs.kernel.org/en/latest/en/users/drivers/mac80211_hwsim.html)
- [Hashcat : format et traitement WPA/WPA2](https://hashcat.net/wiki/doku.php?id=cracking_wpawpa2)
- [Hostapd : exemples de configuration SAE et PMF](https://chromium.googlesource.com/external/w1.fi/cgit/hostap/+/9c6b0a941672eb54e5a5e11f4d6a699f2aba1709/hostapd/hostapd.conf)
