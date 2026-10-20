# Analyse des résultats et limites

Ce projet relie les mécanismes d’authentification Wi-Fi à des observations concrètes. Une capture, un message d’outil ou une récupération réussie doivent être interprétés avec leurs conditions de réalisation. Cette analyse distingue les résultats documentés de ce qui reste à vérifier.

## Résultats retenus

| Scénario | Observation exploitable | Conclusion retenue |
|---|---|---|
| WPA2-PSK | Une paire EAPOL est extraite et le secret faible du laboratoire est retrouvé par dictionnaire. | La capture permet la vérification hors ligne de candidats ; le secret choisi était prévisible. |
| WPA3-SAE | Aucun hash chargeable n’est produit ; une capture est incomplète et une illustration est incohérente. | Le traitement a échoué, mais la comparaison expérimentale ne permet pas de conclure sur la résistance du protocole. |
| Evil Twin | Le client rejoint le BSSID imposé du clone, configuré avec le secret connu ; la récupération est reproduite. | Une association contrôlée au jumeau est démontrée, sans basculement automatique ni interception complète du trafic. |

## Comprendre la récupération WPA2

Une paire EAPOL exploitable fournit des éléments permettant de vérifier un candidat de phrase secrète. Le dictionnaire ne « déchiffre » donc pas directement AES : il propose des valeurs et recherche celle qui correspond à l’échange. Le secret de test était présent dans cette liste, ce qui explique le succès du scénario.

La conclusion utile pour la défense concerne le choix de la phrase secrète. Une valeur unique et difficile à prévoir réduit l’efficacité de cette approche. Le résultat ne permet pas d’annoncer que tous les réseaux WPA2 sont rapidement compromis.

## Pourquoi la partie WPA3 doit être nuancée

Le protocole SAE est conçu pour empêcher la vérification passive de candidats selon le même principe que WPA2-PSK. Cette propriété explique l’intérêt de WPA3, mais elle ne remplace pas la validation des captures du projet.

Ici, l’absence de hash peut également résulter de données manquantes. Les traces signalent une acquisition très réduite, sans les échanges utiles attendus. Une vue légendée WPA3 reprend aussi une image WPA2. Ces limites empêchent de présenter « aucun hash chargé » comme une preuve complète de résistance. De même, PMF est configuré obligatoire, mais les traces de désauthentification ne suffisent pas à valider son efficacité indépendamment des limites de simulation.

## Ce que montre réellement le jumeau

Deux points d’accès peuvent annoncer le même nom. Dans ce projet, le client est toutefois configuré pour rejoindre le clone, et celui-ci dispose déjà du secret. Le scénario permet d’étudier l’association et l’échange obtenu ; il ne prouve pas qu’un client aurait choisi spontanément le clone, ni qu’un attaquant aurait obtenu une session complète sans connaître le secret. Aucun résultat de phishing ou de relais Internet n’est revendiqué.

## Limites et pistes d’amélioration

`mac80211_hwsim` permet de tester les interactions logicielles, mais les expériences ne mesurent pas la portée, les interférences ou la puissance radio. Les mesures de durée et de débit du rapport présentent des contradictions ; elles sont exclues de cette synthèse. PMKID, Dragonblood et Pixie-Dust relèvent de l’étude théorique, sans validation pratique dans ce projet.

Une suite pertinente consisterait à refaire l’acquisition WPA3 avec des critères de complétude explicites, puis à vérifier le comportement d’association sur un matériel de laboratoire. Les recommandations portent sur WPA3-SAE avec PMF, des secrets robustes, des équipements à jour, la désactivation des fonctions inutiles et la surveillance des points d’accès inattendus.

## Références techniques

- [Linux Wireless : fonctionnement du simulateur](https://wireless.docs.kernel.org/en/latest/en/users/drivers/mac80211_hwsim.html)
- [Hashcat : vérification WPA/WPA2](https://hashcat.net/wiki/doku.php?id=cracking_wpawpa2)
- [Hostapd : configuration WPA3-Personal et PMF](https://chromium.googlesource.com/external/w1.fi/cgit/hostap/+/9c6b0a941672eb54e5a5e11f4d6a699f2aba1709/hostapd/hostapd.conf)
