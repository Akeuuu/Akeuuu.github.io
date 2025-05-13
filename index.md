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
- Make
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
./raytracer <FICHIER_SCENE> -SFML(optionnel)
```

Où `<FICHIER_SCENE>` est le chemin vers votre fichier de configuration de scène (format libconfig++).

L'image générée sera sauvegardée au format PPM.

## Format du fichier de scène

Le programme utilise des fichiers de configuration au format libconfig++ pour définir les scènes à rendre. Voici un exemple de structure:

(
  NB/ les parties suivantes sont indispensables même vide : 
    - camera
    - primitives
    - objects
    - scenes
    - lights
)

```
// Configuration of the camera
camera : {
    resolution = { width = 1920; height = 1080; };
    origin = { x = 0; y = -100; z = 20; };
    vector = { x = 1; y = 1; z = 1; };
    fieldOfView = 72.0;
};

objects : {
    obj = (
        {
            path = "Objects/mita.obj";
        }
        {
            path = "Objects/teapot.obj";
        }
    );
};

scenes : {
    scene = (
        {
            path = "otherscene.cfg";
        },
        {
            path = "scene2.cfg";
        }
    );

    Pour plusieurs scènes :
    scene_list = (
        {
            path = "scene1.cfg";
        },
        {
            path = "scene2.cfg";
        }
    );
};

// Primitives in the scene
primitives : {
    // List of spheres
    plane = (
        {
            axis = "Z";
            position = -20;
            color = { r = 64; g = 64; b = 255; };
        }
    );
};

// Light configuration
lights : {
    ambientlight = (
        {
            intensity = 0.1;
            color = { r = 255; g = 255; b = 255; };
        }
    );

    directionallight = (
        {
            intensity = 0.2;
            direction = { x = 10; y = 10; z = -10; };
            color = { r = 140; g = 255; b = 255; };
        }
    );
    pointlight = (
        {
            origin = { x = 0; y = -35; z = 20; };
            intensity = 1.0;
            color = { r = 255; g = 255; b = 255; };
        }
    );
    falcutatives options : color = { r = 255, g = 255, b = 255 };
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

## Architecture

### Patrons de conception

- Factory
- Builder

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

- Multithreading 

## Comment ajouter une nouvelle primitive

L’ajout d’une primitive dans le raytracer se fait en plusieurs étapes essentielles :

---

### Étape 1 — Ajouter le parsing

Ajoute la lecture de ta nouvelle primitive dans le fichier :

📂 `Utils/Parser/parser.cpp`

Cela permettra d’interpréter les données depuis le fichier `.cfg` de scène.

---

### Étape 2 — Créer la classe de la primitive

Crée un nouveau dossier pour ta primitive dans :

📂 `Primitives/NomDeTaPrimitive/`

Et ajoute un fichier `NomDeTaPrimitive.cpp` contenant :

- Un constructeur qui initialise les champs à partir des données du `.cfg`.
- Une méthode :

```cpp
DataHits hits(const RayTracer::Ray& ray);

using DataHits = std::tuple<double, Math::Point3D, Math::Vector3D>;
```

### Ce tuple contient :

    double : la distance entre la caméra et le point d’intersection

    Math::Point3D : la position du point touché

    Math::Vector3D : la normale à la surface au point d’impact

Une fois ces deux étapes terminées, ta primitive est prête à être utilisée dans une scène .cfg !