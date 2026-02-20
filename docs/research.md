# Étude Technique : Interfaçage Matériel (OpenHardwareMonitor)

Ce document synthétise l'analyse technique effectuée avant l'implémentation du moteur de monitoring.

## 1. Choix de la Bibliothèque
Après comparaison, la bibliothèque **OpenHardwareMonitor (OHM)** a été retenue face à WMI (Windows Management Instrumentation) pour sa capacité à accéder directement aux registres des capteurs (températures GPU et CPU spécifiques) sans dépendre de drivers tiers limités.

## 2. Défis de Programmation Système

### Accès de niveau Ring 0
L'accès aux capteurs physiques nécessite des privilèges élevés pour interagir avec le matériel.
* **Impact :** L'application doit impérativement s'exécuter avec les droits d'administrateur.
* **Solution :** Intégration d'un `app.manifest` configuré avec `requireAdministrator`.

### Latence de lecture des capteurs
La lecture de certains capteurs (notamment via le bus I2C ou SMBus) est une opération bloquante.
* **Analyse :** Un appel synchrone sur le thread principal de WPF gèlerait l'interface pendant plusieurs millisecondes.
* **Stratégie Engine :** Mise en place d'une boucle de rafraîchissement asynchrone pour décorréler la fréquence de lecture des capteurs de la fréquence de rafraîchissement de la vue.

## 3. Structure des Données Cibles
| Composant | Données prioritaires |
| :--- | :--- |
| **CPU** | Core Temp, Package Power, Clock Speed. |
| **GPU** | Core Temp, Hotspot, Memory Usage (VRAM). |
| **RAM** | Used Memory, Load Percentage. |
