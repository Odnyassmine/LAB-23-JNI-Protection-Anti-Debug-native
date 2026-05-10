# JNIDemo - Laboratoire de Sécurité Native

Ce projet illustre l'intégration de mécanismes défensifs dans une bibliothèque native Android (C++) via JNI. L'objectif est de détecter des environnements d'analyse ou de débogage pour protéger les fonctions sensibles de l'application.

## Fonctionnalités

### 1. Fonctions JNI Classiques
*   **Hello World Natif** : Une simple chaîne de caractères passée du C++ vers Java.
*   **Calcul de Factoriel** : Démonstration de calcul mathématique performant en natif.

### 2. Couche Défensive Native
L'application intègre une méthode `isDebugDetected()` qui effectue deux contrôles bas niveau :

*   **Détection de Traçage (`ptrace`)** : Tente d'utiliser `PTRACE_TRACEME`. Si l'appel échoue, cela indique généralement qu'un débogueur (comme LLDB ou GDB) est déjà attaché au processus.
*   **Inspection Mémoire (`/proc/self/maps`)** : Analyse les bibliothèques chargées en mémoire à la recherche de signatures liées à l'instrumentation dynamique ou au root (Frida, Xposed, Magisk, gdbserver).

## Architecture du Projet

*   `app/src/main/cpp/native-lib.cpp` : Contient toute la logique C++ et les points d'entrée JNI.
*   `app/src/main/cpp/CMakeLists.txt` : Script de configuration pour la compilation de la bibliothèque `libnative-lib.so`.
*   `app/src/main/java/com/example/jnidemo/MainActivity.java` : Gère le chargement de la bibliothèque et adapte l'UI selon le verdict de sécurité.
*   `app/src/main/res/layout/activity_main.xml` : Interface utilisateur affichant le statut de sécurité.

## Configuration Requise

*   Android Studio
*   Android NDK (Side-by-side)
*   CMake
*   Appareil Android ou Émulateur

## Installation et Utilisation

1.  **Clonage / Ouverture** : Ouvrez le projet dans Android Studio.
2.  **Synchronisation Gradle** : Assurez-vous que le projet se synchronise correctement (nécessite la configuration du NDK).
3.  **Exécution Normale** : Lancez l'application normalement. Vous devriez voir "Etat securite : OK" et les fonctions natives activées.
4.  **Exécution en Debug** : Lancez l'application en mode **Debug** (icône scarabée). Le système peut alors détecter le traçage et afficher une alerte rouge.

## Observation des Logs

Le code natif utilise la bibliothèque `android/log.h` pour journaliser ses actions. Pour suivre les vérifications en temps réel :
1. Ouvrez l'onglet **Logcat** dans Android Studio.
2. Filtrez par le tag : `ANTI_DEBUG`.

Exemple de sortie :
```text
I/ANTI_DEBUG: Aucun trace/debug detecte via ptrace
I/ANTI_DEBUG: Aucune signature suspecte trouvee dans /proc/self/maps
I/ANTI_DEBUG: Etat de securite : OK
```


