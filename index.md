# mypandoc: A Simplified Document Converter

## Introduction

`mypandoc` est une version simplifiée de Pandoc, un convertisseur de documents populaire et open-source. Ce programme permet de convertir des documents d'un format à un autre, en prenant en charge les formats XML, JSON et Markdown. Il est écrit en Haskell et utilise une bibliothèque de parsing personnalisée pour gérer la conversion.

## Fonctionnalités

Le programme prend un fichier en entrée et le convertit dans un autre format. Il prend en charge les formats suivants :
- XML
- JSON
- Markdown

### Structure du Document

Un document est composé de deux parties principales : l'en-tête (header) et le corps (content).

1. **En-tête** :
   - **Title** : Le titre du document.
   - **Author** (facultatif) : L'auteur du document.
   - **Date** (facultatif) : La date de création du document.

2. **Contenu** :
   - **Texte** : Séquence de caractères ASCII.
   - **Formatage** : 
     - Italique
     - Gras
     - Code
   - **Liens et Images** : 
     - Lien : Composé de texte et contenu supplémentaire.
     - Image : Composé de texte et contenu supplémentaire.
   - **Éléments structurels** :
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

