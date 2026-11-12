# ShabdGuru

ShabdGuru is a Marathi-language text prediction and transliteration project that combines n-gram language modeling with semantic understanding to improve next-word suggestions for Marathi text. It uses a lightweight Flask web app and a manually curated semantic dictionary to handle words that may not appear directly in the training corpus.

## Overview

The project aims to predict likely next words in Marathi sentences by learning from a corpus and enhancing suggestions with semantic relationships such as:

- health-related words like `बरा`, `आजारी`, `बीमार`
- emotion words like `खुश`, `दुःखी`, `राग`
- state and condition words like `थकलो`, `तयार`, `व्यस्त`
- weather and food-related context

This allows meaningful suggestions even when the exact word combination is absent from the corpus. For example:

- `मी बरा नाही` → suggested next word: `आहे`
- The app relates words through semantic groups, synonyms, and character-level similarity to infer likely continuations.

## Features

- Marathi next-word prediction using bigram and trigram models
- semantic enhancement using a handcrafted dictionary of related words
- synonym mapping to improve contextual understanding
- character-level similarity fallback for unseen or rare words
- transliteration support from Romanized input to Marathi script
- web UI for prediction and transliteration

## Tech Stack

- Python
- Flask
- scikit-learn
- NumPy
- pickle for model persistence

## Project Structure

```text
shabd-guru/
├── app.py                  # Flask app and prediction logic
├── build_model.py          # Corpus cleaning and n-gram model builder
├── corpus.txt              # Marathi text corpus used for training
├── marathi_models.pkl      # Serialized trained model files
├── requirements.txt        # Python dependencies
├── templates/
│   ├── home.html           # Prediction/autocomplete page
│   └── translator.html     # Transliteration page
└── README.md
```

## Getting Started

### 1. Create a virtual environment

```bash
python -m venv .venv
source .venv/bin/activate   # Linux/macOS
.venv\Scripts\activate      # Windows
```

### 2. Install dependencies

```bash
pip install -r requirements.txt
```

### 3. Build the model

```bash
python build_model.py
```

This creates `marathi_models.pkl` from the corpus in `corpus.txt`.

### 4. Run the app

```bash
python app.py
```

Then open:

```text
http://localhost:5000/
```

## How It Works

1. The corpus is cleaned and tokenized in `build_model.py`.
2. Bigram and trigram counts are generated into a language model.
3. `app.py` loads the trained model and adds semantic dictionaries for related concepts.
4. When user text is entered, the app:
   - checks direct n-gram matches first
   - adds semantic predictions from related word groups
   - uses similarity-based reasoning for unseen words
   - returns the top suggestions

## Example Usage

### Prediction

Input:

```text
मी बरा नाही
```

Expected suggestion:

```text
आहे
```

### Transliteration

Input (Romanized):

```text
mi bara nahi
```

Output:

```text
मी बरा नाही
```

## API Endpoints

### `POST /suggest`

Accepts JSON payload:

```json
{
  "text": "मी बरा नाही"
}
```

Returns a JSON array of suggested next words.

### `POST /transliterate`

Accepts JSON payload:

```json
{
  "text": "mi bara nahi"
}
```

Returns:

```json
{
  "marathi_text": "मी बरा नाही"
}
```

## Notes

- The semantic dictionary is intentionally lightweight and extendable.
- The project is a research-style prototype focused on Marathi language prediction rather than a production-grade NLP pipeline.
- It can be improved by adding a larger corpus, richer word embeddings, or transformer-based models.

## License

This project is intended for educational and experimental use.
