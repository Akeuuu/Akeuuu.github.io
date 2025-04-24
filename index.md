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

## 📄 Structure de Données

Le programme utilise plusieurs types de données pour représenter la structure d'un document. Ces types sont définis comme suit :

### `Document`

Le type `Document` représente un document complet, comprenant un en-tête et un corps de contenu.

### Structure de Document (Haskell)

-- Représente un document complet, avec un en-tête (header) et un corps (content)
data Document = Document
  { header  :: Header        -- Métadonnées du document
  , content :: [Block]       -- Corps du document composé de blocs
  } deriving (Show, Eq)

-- Le header n'est autre qu'un titre, potentiellement un auteur, et une date
data Header = Header
  { title  :: String         -- Titre du document (obligatoire)
  , author :: Maybe String   -- Auteur (optionnel)
  , date   :: Maybe String   -- Date de création (optionnelle)
  } deriving (Show, Eq)

-- La structure Block représente les éléments de contenu principaux :
-- paragraphes, sections, blocs de code, listes
data Block
  = Paragraph [Inline]             -- Paragraphe avec du texte enrichi (liste d'Inline)
  | Section (Maybe String) [Block] -- Section avec titre optionnel et blocs imbriqués
  | CodeBlock Block                -- Bloc de code, contenu sous forme de Block (souvent un paragraphe)
  | List [Block]                   -- Liste de blocs, similaire à une Section sans titre
  deriving (Show, Eq)

-- Inline représente les éléments textuels à l’intérieur d’un paragraphe
data Inline
  = PlainText String               -- Texte brut
  | Italic Inline                  -- Texte en italique (Inline imbriqué)
  | Bold Inline                    -- Texte en gras (Inline imbriqué)
  | Code Inline                    -- Texte formaté comme du code (Inline imbriqué)
  | Link String [Inline]           -- Lien : URL + description en Inline
  | Image String [Inline]          -- Image : source + texte alternatif
  deriving (Show, Eq)

