# mypandoc: A Simplified Document Converter

## 🌟 Introduction

`mypandoc` est une version simplifiée de **Pandoc**, un convertisseur de documents populaire et open-source. Ce programme permet de convertir des documents d'un format à un autre, prenant en charge les formats suivants : **XML**, **JSON**, et **Markdown**. Écrit en **Haskell**, il utilise une bibliothèque de parsing personnalisée pour gérer la conversion des fichiers.

## 📝 Fonctionnalités

Le programme permet de convertir des documents d'un format à un autre, en suivant cette structure :

- **Formats d'entrée** : XML, JSON, Markdown
- **Formats de sortie** : XML, JSON, Markdown

### Structure d'un Document

Un document est composé de deux parties principales : l'en-tête (**header**) et le corps (**content**).

#### 1. En-tête
- **Title** : Le titre du document.
- **Author** (optionnel) : L'auteur du document.
- **Date** (optionnel) : La date de création du document.

#### 2. Contenu
Le corps du document contient plusieurs éléments :
- **Texte** : Séquence de caractères ASCII.
- **Formatage** : 
  - Italique
  - Gras
  - Code
- **Liens et Images** : 
  - Lien : Composé de texte et contenu supplémentaire.
  - Image : Composé de texte et contenu supplémentaire.
- **Éléments Structurels** :
  - Paragraphe
  - Section
  - Bloc de code
- **Listes** :
  - Liste
  - Élément de liste

### Formats Supportés

- **XML** : Le document est structuré avec des balises `<document>`, `<header>` et `<body>`. Le contenu est placé entre ces balises.

- **JSON** : Le document est représenté par un objet JSON avec des clés `header` et `body`. Le titre est contenu dans la clé `title` sous l'objet `header`, et le corps du document est une liste sous la clé `body`.

- **Markdown** : Le document utilise la syntaxe Markdown. Le titre du document est spécifié en haut du fichier sous forme de métadonnées, suivi du contenu principal.

## ⚙️ Utilisation

### Options en Ligne de Commande

Le programme prend en charge les options suivantes :

- `-i` : Chemin vers le fichier d'entrée (obligatoire)
- `-f` : Format de sortie (obligatoire : xml, json, markdown)
- `-o` : Chemin vers le fichier de sortie (facultatif)
- `-e` : Format du fichier d'entrée (facultatif)

Si des options invalides sont fournies, le programme renverra un code d'erreur 84 et affichera un message d'utilisation.

#### Exemple d'utilisation

```bash
./mypandoc -f markdown -i example/example.xml
