# Méthodologie de Comptage de Tokens pour un Fichier PDF

Ce document explique la méthode correcte pour compter les tokens d'un fichier PDF dans le contexte des modèles de langage (LLMs).

## Le Principe Fondamental

Un modèle de langage ne lit pas directement un fichier PDF. Il lit du **texte**. Par conséquent, pour connaître le nombre de tokens d'un PDF, il faut d'abord en extraire le contenu textuel, puis compter les tokens de ce texte.

Toute tentative de compter les tokens des données binaires brutes d'un fichier PDF est incorrecte et ne représente pas le "coût" ou la "longueur" du contenu pour un LLM.

## Le Processus en 2 Étapes

1.  **Étape 1 : Extraction du Texte**
    - On utilise une bibliothèque spécialisée comme `PyMuPDF` (importée sous le nom `fitz`) pour lire le fichier PDF.
    - On parcourt chaque page du document.
    - Pour chaque page, on appelle la fonction `get_text()` qui se charge d'extraire et de décompresser le contenu textuel visible.
    - On concatène le texte de toutes les pages pour obtenir une seule chaîne de caractères.

2.  **Étape 2 : Comptage des Tokens**
    - On utilise la bibliothèque `tiktoken`, la référence pour la tokenisation compatible avec les modèles d'OpenAI et d'autres.
    - On charge un encodeur spécifique. Le `cl100k_base` est celui utilisé par les modèles modernes comme GPT-4.
    - On passe le texte extrait à la fonction `encode()`.
    - Cette fonction retourne une liste d'entiers, où chaque entier représente un token.
    - La longueur de cette liste (`len()`) nous donne le nombre total de tokens.

## Le Code Python

Voici un script Python fonctionnel qui met en œuvre cette méthode.

```python
import os
import fitz  # PyMuPDF
import tiktoken

def count_tokens_in_pdf(pdf_path):
    """
    Extrait le texte d'un fichier PDF et compte le nombre de tokens
    en utilisant l'encodage 'cl100k_base'.

    Args:
        pdf_path (str): Le chemin vers le fichier PDF.

    Returns:
        int: Le nombre de tokens, ou -1 en cas d'erreur.
    """
    if not os.path.exists(pdf_path):
        print(f"Erreur : Le fichier {pdf_path} n'a pas été trouvé.")
        return -1

    text_content = ""
    try:
        # Étape 1: Extraction du Texte
        doc = fitz.open(pdf_path)
        for page in doc:
            text_content += page.get_text()
        doc.close()

        if not text_content.strip():
            print("Avertissement : Aucun texte n'a pu être extrait du PDF.")
            return 0

        # Étape 2: Comptage des Tokens
        encoding = tiktoken.get_encoding("cl100k_base")
        token_count = len(encoding.encode(text_content))

        return token_count

    except Exception as e:
        print(f"Une erreur est survenue : {e}")
        return -1

# --- Exemple d'utilisation ---
if __name__ == "__main__":
    # Remplacez par le chemin de votre fichier PDF
    pdf_file_path = r"D:\ASL\SUPCOMD\Year3\Stage PFE\Dataset_Collection\My_PDFs\9.Code des ports maritimes.pdf"

    total_tokens = count_tokens_in_pdf(pdf_file_path)

    if total_tokens != -1:
        print(f"Le fichier '{os.path.basename(pdf_file_path)}' contient environ {total_tokens} tokens.")

```
