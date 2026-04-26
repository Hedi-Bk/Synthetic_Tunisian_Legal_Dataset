# Génération de Dataset Juridique avec un LLM Teacher

Ce projet est dédié à la création d'un dataset juridique spécialisé en droit tunisien. Le processus de génération est assisté par un "LLM Teacher" pour produire des exemples de questions-réponses complexes et pertinents.

L'outil principal utilisé pour cette génération est **Google AI Studio** avec le modèle **Gemini 3 Pro**.

## Structure du Dépôt

Ce dépôt contient les éléments suivants :

- **/Batch_Outputs/** : Ce dossier contient l'ensemble des lots (batches) de données générées au format JSON. Chaque fichier représente un lot de paires de questions-réponses basées sur un texte de loi spécifique.
- **Output_Sample.json** : Ce fichier sert de modèle et d'exemple pour la structure JSON attendue pour chaque lot de données.

## Structure des Données

Chaque fichier JSON dans `Batch_Outputs` suit une structure précise pour garantir la cohérence du dataset. Voici les champs principaux :

- `batch_id`: Un identifiant unique pour le lot.
- `source`: Le nom du texte de loi source (ex: "Code des Douanes de Tunisie").
- `articles_covered`: Une liste des numéros d'articles qui ont été utilisés pour générer les exemples dans ce lot.
- `examples`: Une liste d'objets, où chaque objet est un exemple de question-réponse.
  - `example_id`: Un identifiant unique pour l'exemple.
  - `language`: La langue de l'exemple (`fr`, `ar`, `derja`).
  - `category`: La catégorie de raisonnement juridique (ex: `direct`, `multi_article`, `principle_exception`).
  - `messages`: Une conversation structurée entre un `system`, un `user` et un `assistant`.
- `coverage_summary`: Un résumé de la couverture des articles du texte de loi dans le lot.

## Mots-clés

- Legal Tech
- Dataset Juridique
- Génération de Données
- Large Language Model (LLM)
- Gemini
- Google AI Studio
- Droit Tunisien
- Question-Answering (QA)
- Instruction-Following
- Fine-tuning
