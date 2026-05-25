Rapport d'analyse statique - Application Android

Informations générales

Date d'analyse : 19/05/2026  
Analyste : Salma Ait Zidan  
APK analysé : app-debug.apk  

File Information
MD5 : 4a15523a9e071e8fcf44dc2fb20ef378  
SHA1 : f42c1acac2e5eb6491d7d27847c3f11cac094e6c  
SHA256 : d48453794f399f6d4806a94f8e31f935d7fd15154f243c74d07209c50506fc6b  
Size : 10.62 MB  

Application Information
App Name : Projet WS  
Package Name : com.example.projetws  
Main Activity : com.example.projetws.ui.MainActivity  
Min SDK : 24 (Android 7.0)  
Target SDK : 36  
Version Name : 1.0  
Version Code : 1  

Outils utilisés
MobSF (Mobile Security Framework)  
VM Mobexler  

Résumé exécutif

L’analyse statique de l’application Projet WS révèle un niveau de risque élevé.  
Plusieurs vulnérabilités critiques ont été identifiées, notamment l’utilisation de communications réseau non sécurisées via HTTP, l’activation du mode debug en production, ainsi que l’exposition d’adresses IP internes et d’endpoints API dans le code source.  

Bien que l’application ne contienne aucun tracker tiers, son niveau de sécurité global reste insuffisant pour un environnement de production.

Vulnérabilités critiques

1. Communication HTTP non sécurisée
Sévérité : Critique  
MASVS : MSTG-NETWORK-1  
Description : L’application autorise le trafic HTTP via usesCleartextTraffic=true  
Preuve : endpoints http://192.168.0.162/...  
Impact : interception de données (attaque MITM)  
Remédiation : forcer HTTPS et désactiver le trafic HTTP  

2. Debug mode activé
Sévérité : Critique  
MASVS : MSTG-RESILIENCE-1  
Description : L’application est signée avec un certificat debug  
Preuve : analyse MobSF certificate section  
Impact : reverse engineering et accès mémoire  
Remédiation : désactiver debuggable en production  

3. minSdkVersion trop faible
Sévérité : Élevée  
MASVS : MSTG-PLATFORM-1  
Description : Support Android 7.0 (API 24)  
Impact : exposition aux vulnérabilités des anciennes versions Android  
Remédiation : augmenter minSdkVersion à Android 10+  

4. IP Address Disclosure
Sévérité : Élevée  
MASVS : MSTG-CODE-2  
Description : Adresse IP backend exposée dans le code  
Preuve : 192.168.0.162  
Impact : reconnaissance de l’infrastructure  
Remédiation : externaliser la configuration serveur  

5. Hardcoded API endpoints
Sévérité : Élevée  
MASVS : MSTG-STORAGE-14  
Description : Endpoints API présents dans le code source  
Preuve : createEtudiant.php / loadEtudiant.php  
Impact : reverse engineering facilité  
Remédiation : centraliser la configuration API  

6. Broadcast Receiver exporté
Sévérité : Moyenne  
MASVS : MSTG-PLATFORM-2  
Description : Composant accessible par d’autres applications  
Impact : exploitation inter-app possible  
Remédiation : restreindre exported=false  

Autres observations

- Logging potentiel de données sensibles  
- Génération de nombres aléatoires non sécurisée  
- Absence de chiffrement sur certaines communications  

Bon point de sécurité

SSL Certificate Pinning est présent  
Cela protège les communications HTTPS contre les attaques de type MITM  

Évaluation globale

Score de sécurité : 40/100  
Niveau de risque : Élevé  
Statut : Non prêt pour la production  

Recommandations prioritaires

1. Forcer HTTPS sur toutes les communications réseau  
2. Désactiver le mode debug en production  
3. Supprimer les endpoints codés en dur  
4. Sécuriser les composants exportés  
5. Augmenter la version minimale Android supportée  
