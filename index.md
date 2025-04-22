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

- **XML** : Le document est structuré avec des balises `<document>`, `<header>` et `<body>`.
  Exemple :
  ```xml
  <document>
    <header title="Simple example"></header>
    <body>
      <paragraph>This is a simple example</paragraph>
    </body>
  </document>
