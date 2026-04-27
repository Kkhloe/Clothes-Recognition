Here's a clean, professional, and comprehensive **README.md** for your Fashion Product Attribute Extraction Pipeline:

---

# Fashion Product Attribute Extraction Pipeline

A robust multilingual pipeline for extracting **style, color, material, composition, and visual patterns** from fashion product data (titles + descriptions).

Designed for e-commerce platforms operating across multiple European and Asian markets (Czech, Slovak, Greek, Hungarian, Slovenian, Lithuanian, Latvian, Croatian, Bulgarian, Estonian, Turkish, Romanian, Spanish, etc.).

---

## ✨ Features

- **Style Prediction**: Zero-shot semantic matching using sentence embeddings + rule boosting
- **Color Extraction**: Multilingual keyword matching + KeyBERT candidate scoring
- **Material & Composition**: Rule-based + NER (Named Entity Recognition) with percentage parsing
- **Visual Pattern Detection**: Regex + KeyBERT + multilingual BERT embedding distillation
- **Multilingual Support**: Automatic translation & alias generation for 13+ languages
- **High Performance**: GPU-accelerated (T4 optimized), batch processing, progress tracking
- **Robust & Production-ready**: Caching, error handling, deduplication, text cleaning

---

## 📋 Requirements

### Files needed in the project root:
- `aliases_seed_en.json` — English style aliases seed (required)
- `train_with_predictions.csv` — Training data
- `test_with_predictions.csv` — Test data

### Python Dependencies
```bash
ftfy, deep-translator, unidecode, sentence-transformers, keybert, transformers, torch, pandas, tqdm
```

All dependencies are automatically installed by the notebook.

---

## 📁 Project Structure

```
.
├── Extractions.ipynb                 # Main pipeline notebook
├── aliases_seed_en.json              # Style seed data (EN)
├── aliases_multilang.json            # Auto-generated multilingual aliases
├── style_to_glami_names.json         # Compiled regex + negatives
├── outputs/
│   ├── enriched_train.csv
│   └── enriched_test.csv
├── .cache/
│   └── translate_cache.json          # Translation cache
└── README.md
```

---

## 🚀 How to Run

1. **Place required files**:
   - Put `aliases_seed_en.json`, `train_with_predictions.csv`, and `test_with_predictions.csv` in the root directory.

2. **Open the notebook**:
   ```bash
   jupyter notebook Extractions.ipynb
   ```

3. **Run all cells** (recommended in order).

The pipeline will:
- Generate multilingual aliases (if not present)
- Build style matching rules
- Predict **style**, **color**, **materials**, **composition**, and **patterns**
- Save enriched datasets to `outputs/enriched_train.csv` and `outputs/enriched_test.csv`

---

## 📊 Output Columns Added

| Column                        | Description |
|------------------------------|-----------|
| `pred_style`                 | Predicted fashion style |
| `pred_conf`                  | Confidence score for style |
| `primary_color`              | Main color |
| `primary_color_highconf`     | High-confidence primary color |
| `colors`                     | All detected colors |
| `colors_highconf`            | High-confidence colors |
| `materials_final`            | List of extracted materials |
| `composition`                | Material composition with percentages |
| `materials_final_str`        | Pipe-separated materials string |
| `composition_str`            | Formatted composition string |
| `patterns`                   | Detected visual patterns (striped, floral, etc.) |

---

## ⚙️ Key Configuration Parameters

Located at the top of the notebook:

- `STYLE_BATCH_SIZE`, `NER_BATCH_SIZE` — Optimized for Tesla T4 GPU
- `NER_SCORE_THRESHOLD`, `COLOR_SCORE_THRESHOLD`, `PATTERN_THRESHOLD`
- `STYLE_RULE_BOOST`, `STYLE_USE_NEGATIVES`
- Language mappings in `GEO_TO_LANG`

---

## 🛠 Technical Highlights

- **Text Cleaning**: `ftfy` + regex + unicode normalization
- **Style Model**: `paraphrase-multilingual-MiniLM-L12-v2`
- **NER Model**: `xlm-roberta-base-ner-hrl` (with multilingual BERT fallback)
- **Translation**: Google Translator with caching
- **Pattern Detection**: Hybrid approach (rules + KeyBERT + embedding similarity)
- **Efficiency**: Deduplication of processing, smart caching, progress bars with ETA

---

## 🔄 Pipeline Stages

1. **Multilingual Alias Generation**
2. **Style → GLAMI Name Compilation** (regex + negatives)
3. **Zero-shot Style Prediction** (embedding + rules)
4. **Material & Composition Extraction** (rule + NER)
5. **Color Extraction** (keyword + KeyBERT)
6. **Visual Pattern Extraction** (regex + KeyBERT + BERT)

---

## 📝 Notes

- The first run may take longer due to model downloads and translation caching.
- Translation cache is saved to `.cache/translate_cache.json` for reuse.
- GPU is highly recommended (Tesla T4 or better). CPU fallback is supported but significantly slower.
- The pipeline is designed to be robust against messy e-commerce data (HTML tags, encoding issues, mixed languages).

---

## 📄 License

This project is for internal use / research purposes.

---

**Made with ❤️ for fashion tech**

---

Would you like me to also create a shorter version (for GitHub repo overview) or add a **Usage Tips & Troubleshooting** section?
