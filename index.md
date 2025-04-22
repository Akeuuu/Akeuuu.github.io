---
title: "Documentation Mypandoc"
---

# Mypandoc — Documentation Technique

Bienvenue dans la documentation du projet **Mypandoc**.  
Ce site a été généré avec [GitHub Pages](https://pages.github.com/) en Markdown.

---

## 📦 Présentation du projet

**Mypandoc** est un outil de conversion de documents basé sur un parseur Haskell personnalisé.  
Il supporte la transformation de fichiers XML vers une structure de document type `Document`, `Header`, `Block`, et `Inline`.

---

## 📁 Arbores

---

## 🛠️ Fonctions clés

- `xmlToDocument :: XmlValue -> Maybe Document`
- `valueToBlock :: XmlValue -> Maybe Block`
- `valuesToInlines :: [XmlValue] -> Maybe [Inline]`

---

## 🔍 Objectifs

- ✅ Parser XML custom
- ✅ Conversion vers un format de document type Markdown
- ⏳ Ajout d’un support JSON
- ⏳ Génération PDF via pandoc

---

## ✍️ Auteurs

- Max Roiron  
- Projet réalisé dans le cadre de l’Epitech

---

## 📄 Licence

Projet sous licence MIT — libre d’usage, modification et diffusion.
