# RAYTRACER - VOTRE CPU FAIT BRRRRR!

Un moteur de ray tracing haute performance construit en C++ qui simule le chemin inverse de la lumière pour générer des images numériques réalistes.

![Exemple de RayTracer](https://via.placeholder.com/800x450)

## Table des matières

1. [Aperçu](#aperçu)
2. [Installation](#installation)
3. [Utilisation](#utilisation)
4. [Format du fichier de scène](#format-du-fichier-de-scène)
5. [Fonctionnalités](#fonctionnalités)
   - [Fonctionnalités essentielles](#fonctionnalités-essentielles)
   - [Fonctionnalités avancées](#fonctionnalités-avancées)
   - [Améliorations optionnelles](#améliorations-optionnelles)
6. [Architecture](#architecture)
   - [Patrons de conception](#patrons-de-conception)
   - [Structure des interfaces](#structure-des-interfaces)
   - [Système de plugins](#système-de-plugins)
7. [Développement](#développement)
8. [Exemples](#exemples)
9. [Optimisations de performance](#optimisations-de-performance)
10. [Dépannage](#dépannage)

## Aperçu

Le projet RayTracer est un moteur de rendu par lancer de rayons permettant de générer des images réalistes en simulant la trajectoire inverse de la lumière. Le programme permet de charger une scène à partir d'un fichier de configuration et de produire une image de haute qualité.

## Installation

### Prérequis

- GCC/G++ avec support C++17 ou supérieur
- Make ou CMake
- Bibliothèque libconfig++
- SFML (pour l'affichage optionnel)

### Compilation avec Make

```bash
# Compilation du projet
make

# Nettoyer les fichiers objets
make clean

# Nettoyer les fichiers objets et l'exécutable
make fclean

# Recompiler le projet
make re
```

## Utilisation

```bash
./raytracer <FICHIER_SCENE>
```

Où `<FICHIER_SCENE>` est le chemin vers votre fichier de configuration de scène (format libconfig++).

L'image générée sera sauvegardée au format PPM.

## Format du fichier de scène

Le programme utilise des fichiers de configuration au format libconfig++ pour définir les scènes à rendre. Voici un exemple de structure:

```
# Configuration de la caméra
camera:
{
  resolution = { width = 1920; height = 1080; };
  position = { x = 0; y = -100; z = 20; };
  rotation = { x = 0; y = 0; z = 0; };
  fieldOfView = 72.0; # En degrés
};

# Primitives dans la scène
primitives:
{
  # Liste des sphères
  spheres = (
    { x = 60; y = 5; z = 40; r = 25; color = { r = 255; g = 64; b = 64; }; },
    { x = -40; y = 20; z = -10; r = 35; color = { r = 64; g = 255; b = 64; }; }
  );

  # Liste des plans
  planes = (
    { axis = "Z"; position = -20; color = { r = 64; g = 64; b = 255; }; }
  );
};

# Configuration des lumières
lights:
{
  ambient = 0.4; # Multiplicateur de lumière ambiante
  diffuse = 0.6; # Multiplicateur de lumière diffuse

  # Liste des lumières ponctuelles
  point = (
    { x = 400; y = 100; z = 500; }
  );

  # Liste des lumières directionnelles
  directional = ();
};
```

## Fonctionnalités

### Fonctionnalités essentielles

#### Primitives obligatoires
- ✓ Sphère
- ✓ Plan

#### Transformations obligatoires
- ✓ Translation

#### Lumières obligatoires
- ✓ Lumière directionnelle
- ✓ Lumière ambiante

#### Matériaux obligatoires
- ✓ Couleur unie

#### Configuration de scène obligatoire
- ✓ Ajout de primitives à la scène
- ✓ Configuration de l'éclairage
- ✓ Configuration de la caméra

#### Interface obligatoire
- ✓ Sortie vers un fichier PPM (sans interface graphique)

### Fonctionnalités avancées

#### Primitives recommandées
- ✓ Cylindre
- ✓ Cône

#### Transformations recommandées
- ✓ Rotation

#### Lumières recommandées
- ✓ Ombres portées

### Améliorations optionnelles

#### Primitives optionnelles
- Cylindre limité (0.5 point)
- Cône limité (0.5 point)
- Tore (1 point)
- Tanglecube (1 point)
- Triangles (1 point)
- Fichier .OBJ (1 point)
- Fractales (2 points)
- Ruban de Möbius (2 points)

#### Transformations optionnelles
- Échelle (0.5 point)
- Cisaillement (0.5 point)
- Matrice de transformation (2 points)
- Graphe de scène (2 points)

#### Lumières optionnelles
- Plusieurs lumières directionnelles (0.5 point)
- Plusieurs lumières ponctuelles (1 point)
- Lumière colorée (0.5 point)
- Modèle de réflexion de Phong (2 points)
- Occlusion ambiante (2 points)

#### Matériaux optionnels
- Transparence (0.5 point)
- Réfraction (1 point)
- Réflexion (0.5 point)
- Texture depuis un fichier (1 point)
- Texture procédurale en damier (1 point)
- Texture procédurale avec bruit de Perlin (1 point)
- Normal mapping (2 points)

#### Configuration de scène optionnelle
- Import d'une scène dans une scène (2 points)
- Anti-aliasing par suréchantillonnage (0.5 point)
- Anti-aliasing par suréchantillonnage adaptatif (1 point)

#### Optimisations optionnelles
- Partitionnement spatial (2 points)
- Multithreading (1 point)
- Clustering (3 points)

#### Interface optionnelle
- Affichage de l'image pendant et après la génération (1 point)
- Sortie pendant ou après la génération (0.5 point)
- Aperçu de la scène avec un moteur de rendu basique et rapide (2 points)
- Rechargement automatique de la scène lors d'un changement de fichier (1 point)

## Architecture

### Patrons de conception

Le projet implémente au moins deux des patrons de conception suivants:
- Factory
- Builder
- Composite
- Decorator
- Observer
- State
- Mediator

### Structure des interfaces

Pour permettre l'extensibilité, le projet utilise des interfaces au moins pour les primitives et les lumières.

### Système de plugins

Un système de plugins optionnel permet d'étendre les fonctionnalités du moteur de rendu sans réécrire le code. Les plugins sont des bibliothèques dynamiques (.so) chargées au moment de l'exécution.

Les plugins doivent être stockés dans le répertoire `./plugins/` et l'exécutable ne doit pas être lié directement à ces plugins.

Domaines possibles pour les plugins:
- Primitives
- Lumières
- Chargeurs de scènes
- Interface utilisateur graphique
- Moteurs de rendu principaux
- Effets optiques
- Etc.

## Développement

### Bibliothèques autorisées

Les seules bibliothèques autorisées pour les fonctionnalités obligatoires sont:
- La bibliothèque standard C++
- libconfig++ (pour l'analyse des fichiers de configuration de scène)
- SFML (pour l'affichage)

Des bibliothèques supplémentaires peuvent être utilisées pour les fonctionnalités bonus avec l'accord de l'équipe pédagogique.

### Gestion des erreurs

Les messages d'erreur doivent être écrits sur la sortie d'erreur, et le programme doit se terminer avec le code d'erreur 84 (0 s'il n'y a pas d'erreur).

## Exemples

Organisation recommandée pour les démos et captures d'écran:

```
./scenes/
  demo_cone.cfg
  demo_cylinder.cfg
  demo_plane.cfg
  demo_sphere.cfg
  light_point.cfg
  light_directional.cfg
  ...

./screenshots/
  demo_cone.ppm
  demo_cylinder.ppm
  demo_plane.ppm
  demo_sphere.ppm
  light_point.ppm
  light_directional.ppm
  ...
```

## Optimisations de performance

Plusieurs techniques d'optimisation peuvent être implémentées pour améliorer les performances:
- Multithreading pour exploiter pleinement les processeurs multi-cœurs
- Partitionnement spatial pour réduire le nombre de tests d'intersection
- Clustering pour le calcul distribué
- Techniques d'anti-aliasing optimisées

## Dépannage

### Problèmes courants
- **Erreur de segmentation**: Vérifiez la gestion de la mémoire et les limites des tableaux
- **Artefacts visuels**: Vérifiez les calculs d'intersection et de normales
- **Performance médiocre**: Envisagez d'implémenter des optimisations comme le multithreading