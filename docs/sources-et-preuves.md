# Sources et traçabilité des observations

La présentation est fondée sur le rapport de PFA « Pentesting et Audit de Sécurité (Approche Offensive) », année universitaire 2025–2026, et sur les figures qu’il contient. Otmane Arajdal présente ici le projet dans son portfolio. La répartition individuelle des tâches n’est pas établie par les éléments fournis.

Le rapport original compte 54 pages. Les références ci-dessous sont les positions dans le PDF, et non les folios imprimés. Le PDF original et ses captures complètes restent hors de ce dépôt. Les visuels publics sont des schémas et des tableaux de synthèse créés pour la présentation, sans être présentés comme des captures originales.

| Observation | Pages du PDF source | Portée de la preuve |
|---|---|---|
| PFA de 4e année, 2025–2026 | 1 et 3 | Contexte académique |
| Quatre radios virtuelles, outils et rôles | 23, 32 et 43 | Environnement de simulation |
| Connexion WPA2 et observation EAPOL | 34 à 36 | Association de test et capture |
| Conversion d’une paire EAPOL | 37 | Entrée utilisable par le traitement WPA2 |
| Récupération du secret faible de test | 38 | Résultat du cas de démonstration |
| Paramètres SAE et PMF | 38 et 39 | Configuration décrite, sans validation complète de PMF |
| Aucun hash extrait en WPA3, trames manquantes | 41 et 42 | Échec du traitement et insuffisance de la capture |
| Deux BSSID avec le même SSID | 44 et 45 | Duplication du nom de réseau |
| BSSID du jumeau imposé et secret connu du test | 44 et 46 | Association pilotée, pas roaming spontané |
| Extraction et récupération reproduites | 47 et 48 | Résultat sous les conditions du jumeau configuré |
| Limites radio et perspectives | 50 et 53 | Validation matérielle future |

## Précisions apportées à la lecture

- La figure présentée comme une reconnaissance WPA3 à la page 40 réutilise une image WPA2. L’identification SAE apparaît dans une autre vue à la page 41 ; seule cette dernière est pertinente pour cette identification.
- La conversion WPA3 annonce seulement quatre paquets et des trames manquantes. Un fichier vide ne suffit pas à valider toutes les propriétés cryptographiques ou la capture complète.
- Les temps et vitesses cités dans le rapport ne sont pas repris comme un benchmark ; les valeurs ne sont pas cohérentes et aucun journal complet n’est fourni.
- La configuration PMF et les valeurs `0 ACK` doivent être distinguées d’une preuve de blocage d’une désauthentification.
- Le scénario jumeau utilise un client guidé par BSSID et des paramètres connus. Aucun phishing, MITM complet ou choix automatique fondé sur le signal n’est démontré.

Ces précisions décrivent la portée des résultats ; elles ne constituent pas de nouveaux essais.
