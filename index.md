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

```haskell
-- Represents a full document
data Document = Document
  { header  :: Header
  , content :: [Block]
  } deriving (Show, Eq)

La structure Document contient le Header et le body (content) détaillés précédemment.

-- Metadata at the top of the document
data Header = Header
  { title  :: String
  , author :: Maybe String
  , date   :: Maybe String
  } deriving (Show, Eq)

Le header n'est autre qu'un titre et potentiellement un autheur et suivis d'une date


data Block
  = Paragraph [Inline]       -- Regular paragraph
  | Section (Maybe String) [Block]  -- Optional title, nested blocks
  | CodeBlock Block         -- Code block as string
  | List [Block]              -- List of items
  deriving (Show, Eq)

La structure block est celle qui représente les paragraphes/section/code block/list

Paragraphes ([Inline] -> liste de string avec ses spécificités ex : italique / grasse ou un lien)
Section ((Maybe String) -> pour le titre de la section
         [Block] -> est une liste de block elle même car une secton contient plusieurs block)
Code Block (Block -> forcément un paragraphe)
List    ([Block] -> similaire à une section sans maybe string)

data Inline
  = PlainText String
  | Italic Inline
  | Bold Inline
  | Code Inline
  | Link String [Inline]    -- href + description
  | Image String [Inline]   -- src + alt text
  deriving (Show, Eq)

Plaintext (String -> pour du texte)
Italic (Inline -> pour spécifier un autre type de texte ou du texte)
Bold (Inline -> pour spécifier un autre type de texte ou du texte)
Code (Inline -> pour spécifier un autre type de texte ou du texte)
Link (String -> url
      [Inline] -> pour spécifier un autre type de texte ou du texte)
Image (String -> src
       [Inline] -> pour spécifier un autre type de texte ou du texte)
