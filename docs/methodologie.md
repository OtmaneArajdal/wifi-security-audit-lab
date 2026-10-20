# Méthodologie et périmètre

Le travail suit une démarche d’audit dans un laboratoire appartenant aux participants. Les résultats proviennent du rapport de PFA et de ses captures, sans nouvelle exécution réalisée pour cette présentation publique.

## Préparation

La première étape consiste à attribuer les rôles aux quatre interfaces virtuelles, préparer le point d’accès et le client, puis vérifier leur état. La maquette utilise hostapd et wpa_supplicant. Le secret volontairement faible sert à tester une hypothèse connue ; il ne représente pas une information découverte sur un réseau tiers.

## Observation et qualité des données

L’observation relève le protocole annoncé, le SSID, le BSSID, le canal et les échanges d’authentification. Ces champs doivent être reliés au scénario effectivement lancé. Il ne suffit pas de retrouver une image de tableau airodump-ng pour prouver qu’elle correspond au bon protocole.

Le cas WPA2 documente une paire EAPOL exploitable. Le cas WPA3 montre un traitement sans hash écrit, mais aussi une capture ne contenant que quatre paquets et des avertissements de trames manquantes. Ce défaut de données est une limite expérimentale, distincte du principe de sécurité de SAE.

## Vérification de candidats

Pour le cas WPA2, la conversion et le test hors ligne permettent de vérifier le secret de démonstration parmi des candidats. Le mode Hashcat 22000 décrit les enregistrements WPA-PBKDF2-PMKID+EAPOL. Cette capacité ne signifie pas que le chiffrement AES a été cassé.

Les durées de récupération et les débits cités à différents endroits du rapport ne sont pas cohérents entre eux. Ils ne sont donc pas repris comme une mesure de performance. Le nombre d’entrées d’un dictionnaire n’est pas le nombre d’entrées effectivement évaluées avant un résultat.

## Lecture du scénario jumeau

Le même nom de réseau et des BSSID différents illustrent une duplication du SSID. Dans la maquette, le BSSID du jumeau est explicitement renseigné dans le profil du client et le secret partagé est déjà connu dans la configuration. L’association est contrôlée ; les résultats ne démontrent pas un déplacement spontané vers un point d’accès inconnu.

## Reproduction et éléments manquants

La documentation publique permet d’étudier la démarche et les observations. Elle ne contient pas les configurations finales complètes, les captures réseau originales ni les fichiers de hash. Une reproduction devrait figer les versions, documenter les commandes et configurations propres au laboratoire, synchroniser les captures et conserver un journal d’essai distinct pour chaque cas.

Les essais d’injection et de désauthentification rapportés sont limités au simulateur. Aucun test sur un réseau de proximité n’est proposé ou exécuté. Pour une extension matérielle, le périmètre devrait rester un banc de test contrôlé.

Références : [Linux Wireless](https://wireless.docs.kernel.org/en/latest/en/users/drivers/mac80211_hwsim.html) et [Hashcat](https://hashcat.net/wiki/doku.php?id=cracking_wpawpa2).
