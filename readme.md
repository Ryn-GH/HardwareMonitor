# 🖥️ Hardware Monitor - Dashboard Desktop (WPF)

Un outil de monitoring matériel moderne développé en **C# / WPF** permettant de visualiser en temps réel les performances système (CPU, GPU, RAM, Températures). Ce projet explore l'interfaçage système bas niveau et l'optimisation des performances UI.

> **Status : 🚧 Work In Progress**
> Le projet est actuellement en phase de développement actif. L'architecture MVVM est en place et la phase de R&D sur l'accès aux sondes matérielles est terminée.

---

## Fonctionnalités prévues
* **Visualisation Temps Réel :** Affichage des fréquences, tensions et températures via la bibliothèque `OpenHardwareMonitor`.
* **Interface Moderne :** Design fluide conçu avec WPF et les principes MVVM pour une séparation nette entre logique et présentation.
* **Logging :** Système d'exportation des métriques pour analyse de stabilité.

## Stack Technique
* **Langage :** C# (.NET Framework 10).
* **Framework UI :** WPF (Windows Presentation Foundation).
* **Accès Matériel :** LibreHardware Monitor Lib.
* **Architecture :** MVVM (Model-View-ViewModel).

---

## Focus Technique : Réactivité et Multithreading
Dans un moteur de jeu ou un outil de monitoring, l'interface utilisateur (UI) ne doit jamais subir de micro-saccades lors de la récupération de données. 

### Gestion de l'Asynchronisme
Pour garantir une interface fluide (60 FPS+) malgré la latence potentielle des drivers matériels, j'utilise un pattern de **polling asynchrone** :
1. **Background Task :** La lecture des sondes s'exécute sur un thread séparé via `Task.Run`.
2. **Non-Blocking UI :** L'UI est notifiée des changements via l'interface `INotifyPropertyChanged`.
3. **Optimisation :** Utilisation du `Dispatcher` WPF pour synchroniser les mises à jour de données avec le thread de rendu.

---

## Roadmap
- [x] Initialisation de la solution et scaffolding MVVM.
- [x] Création du manifeste pour les privilèges Administrateur.
- [ ] Implémentation du moteur de lecture (OpenHardwareMonitor).
- [ ] Création du Dashboard (Styles XAML personnalisés).
- [ ] Optimisation de l'empreinte mémoire.
