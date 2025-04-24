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
```

# Documentation des Structures de Données de Document

Ce document décrit les types de données Haskell utilisés pour représenter un document structuré avec métadonnées, blocs de contenu et formatage de texte.

## Structure du Document

Représente un document complet avec un en-tête et un contenu.

```haskell
data Document = Document
  { header  :: Header
  , content :: [Block]
  } deriving (Show, Eq)
```

**Champs :**
- `header` - Métadonnées en haut du document (titre, auteur, date)
- `content` - Le contenu principal sous forme de liste de blocs

### `Header`
Contient les métadonnées en-tête du document.

```haskell
data Header = Header
  { title  :: String
  , author :: Maybe String
  , date   :: Maybe String
  } deriving (Show, Eq)
```

**Champs :**
- `title` - Titre du document (obligatoire)
- `author` - Nom de l'auteur (optionnel)
- `date` - Date (optionnelle)

## Blocs de Contenu

### `Block`
Représente les différents types de blocs de contenu.

```haskell
data Block
  = Paragraph [Inline]              -- Paragraphe régulier
  | Section (Maybe String) [Block]  -- Section avec titre optionnel et blocs imbriqués
  | CodeBlock Block                 -- Bloc de code (contient un Block)
  | List [Block]                    -- Liste d'éléments
  deriving (Show, Eq)
```

**Variantes :**
1. **Paragraph**
   - Contient une liste d'éléments `Inline`
   - Représente du texte standard avec formatage

2. **Section**
   - `Maybe String` - Titre de section optionnel
   - `[Block]` - Blocs imbriqués dans la section
   - Peut contenir d'autres blocs de manière récursive

3. **CodeBlock**
   - Contient un seul `Block` représentant le code
   - Contient typiquement un seul paragraphe

4. **List**
   - `[Block]` - Éléments de liste (blocs)
   - Similaire à Section mais sans titre

## Éléments Inline

### `Inline`
Représente les éléments de texte formaté dans les paragraphes.

```haskell
data Inline
  = PlainText String               -- Texte non formaté
  | Italic Inline                  -- Texte en italique
  | Bold Inline                    -- Texte en gras
  | Code Inline                    -- Code inline
  | Link String [Inline]           -- Lien avec URL et description
  | Image String [Inline]          -- Image avec source et texte alternatif
  deriving (Show, Eq)
```

**Variantes :**
1. **PlainText**
   - Texte brut sans formatage

2. **Italic**
   - Encapsule un autre élément `Inline` en italique

3. **Bold**
   - Encapsule un autre élément `Inline` en gras

4. **Code**
   - Encapsule un autre élément `Inline` comme code

5. **Link**
   - `String` - URL/href
   - `[Inline]` - Description formatée du lien

6. **Image**
   - `String` - Chemin/URL de l'image
   - `[Inline]` - Texte alternatif (peut être formaté)

## Notes d'Utilisation

- La structure est récursive : `Block` peut contenir d'autres `Block` (dans les Sections/Listes), et `Inline` peut encapsuler d'autres `Inline`
- Les types Maybe indiquent des champs optionnels
- Tous les types dérivent `Show` et `Eq` pour le débogage et la comparaison
- Le `CodeBlock` contient un `Block` qui serait typiquement un `Paragraph` avec `PlainText`

```markdown
## Implémentation d'Extensions

Pour ajouter un nouveau format (par exemple YAML), vous devez suivre ce pattern :

1. Créer deux modules de conversion :
   ```haskell
   -- Conversion depuis le nouveau format
   YamlToDoc.hs
   -- Conversion vers le nouveau format
   DocToYaml.hs
   ```

2. Implémenter les fonctions principales :
   ```haskell
   -- Dans YamlToDoc.hs
   yamlToDoc :: YamlValue -> Document

   -- Dans DocToYaml.hs
   docToYaml :: Document -> YamlValue
   ```

3. Ajouter le parser dans Parser.hs :
   ```haskell
   parseYaml :: Parser YamlValue
   ```

4. Implémenter la fonction d'écriture :
   ```haskell
   writeYaml :: Document -> String -> IO ()
   writeYaml doc outputPath =
       if outputPath /= ""
           then writeFile outputPath (printYaml (docToYaml doc))
           else putStrLn (printYaml (docToYaml doc))
   ```

5. Ajouter le support dans la fonction principale :
   ```haskell
   pandocYaml :: String -> String -> Conf -> IO ()
   pandocYaml content outputPath conf =
       case runParser parseYaml content of
           Nothing -> putStrLn "Échec de l'analyse YAML" >> exitError
           Just (value, _) ->
               case fromMaybe "" (outForm conf) of
                   "json" -> writeJson (yamlToDoc value) outputPath
                   "xml" -> writeXml (yamlToDoc value) outputPath
                   "markdown" -> writeMd (yamlToDoc value) outputPath
                   "yaml" -> writeYaml (yamlToDoc value) outputPath
                   _ -> exitError
   ```

6. Mettre à jour writePandoc :
   ```haskell
   writePandoc :: Conf -> String -> String -> IO ()
   writePandoc conf content outputPath =
       case fromMaybe "" (inForm conf) of
           "json" -> pandocJson content outputPath conf
           "xml" -> pandocXml content outputPath conf
           "markdown" -> pandocMarkdown content outputPath conf
           "yaml" -> pandocYaml content outputPath conf
           "" -> guessFormat content outputPath conf
           _ -> exitError
   ```

### Architecture Type

Pour chaque nouveau format X, implémenter :

1. XToDoc.hs (conversion vers Document)
2. DocToX.hs (conversion depuis Document)
3. Parser X dans Parser.hs
4. Fonction pandocX suivant le même pattern
5. Fonction writeX pour l'output

Le système est conçu pour que tout nouveau format puisse être converti vers/à partir du type Document, qui sert de format intermédiaire.
```