# 🇩🇪 German Vocabulary Extractor

A production-ready tool for extracting German vocabulary from learning materials (PDF, DOCX, PPTX) with linguistic analysis, OCR support, and intelligent deduplication.

## ✨ Key Features

- **Multi-format support**: PDF (text + image OCR), DOCX, PPTX
- **Linguistic enrichment**:
  - Articles (`der/die/das`) detection from context
  - Plural form generation for nouns
  - Case usage detection (Akkusativ/Dativ/Genitiv) via prepositions
- **Expression extraction**: Multi-word phrases and fixed expressions
- **Strict deduplication**: Normalized lemmatization prevents duplicates
- **Uncertainty handling**: Borderline words stored separately for review
- **Large text optimized**: Streaming processing for books/essays
- **Interactive review**: Summary table before final output
- **Dual output formats**: Clean `.txt` + structured `.csv` with metadata

## 🚀 Quick Start

### Prerequisites
```bash
# Install system dependencies (Ubuntu/Debian)
sudo apt-get install tesseract-ocr tesseract-ocr-deu poppler-utils

# Install Python dependencies
pip install -r requirements.txt
python -m spacy download de_core_news_lg

#or
pip install pdfplumber PyPDF2 python-docx python-pptx Pillow pytesseract spacy pandas tqdm langdetect pattern
python -m spacy download de_core_news_lg



# 1. Install system dependencies
sudo pacman -S --needed tesseract tesseract-data-deu git base-devel

# 2. Remove old venv and create new one
rm -rf venv
python -m venv venv

# 3. Install only necessary packages (NO spacy!)
./venv/bin/pip install --upgrade pip
./venv/bin/pip install PyPDF2 pdfplumber python-docx python-pptx Pillow pytesseract pandas tqdm langdetect

# 4. Run the script
./venv/bin/python german_vocab_extractor_lite.py trans.pdf --force


#skip headers
python3 german_vocab_extractor.py trans.pdf --force

# Basic Usage

# Extract vocabulary from multiple files
python german_vokabel_extractor.py book.pdf exercises.docx presentation.pptx

# Custom minimum word length
python german_vokabel_extractor.py --min-length 4 german_text.pdf

# Specify output directory
python german_vokabel_extractor.py --output-dir my_vocab german_book.pdf

# Output Files Generated

output/
├── vocabulary_20260213_143022.txt      # Clean line-by-line format
├── vocabulary_20260213_143022.csv      # Structured with metadata
├── uncertain_words_20260213_143022.txt # Borderline words for review
├── uncertain_words_20260213_143022.csv
├── expressions_20260213_143022.txt     # Multi-word phrases
└── extraction_report_20260213_143022.json # Processing statistics


# Sample Output

Haus (das) Pl: Häuser
gehen Pl: gehen [Akkusativ]
Buch (das) Pl: Bücher
geben Pl: geben [Dativ, Akkusativ]
Freundschaft (die) Pl: Freundschaften
schnell Pl: schnell
mitbringen Pl: mitbringen [Akkusativ]

# ⚙️ Advanced Configuration

Edit `config.py` to customize:
    - Stop word lists
    - Case preposition mappings
    - Expression detection patterns
    - Confidence thresholds for inclusion

🧪 Edge Cases Handled
Scenario
	
Handling
Scanned PDFs with images
	
Tesseract OCR with German language pack
Mixed-language documents
	
Language detection skips non-German content
Compound German words
	
spaCy compound splitting + lemmatization
Umlauts (äöüß)
	
Full Unicode support in all outputs
Large files (>100MB)
	
Streaming processing with progress bars
Corrupted files
	
Graceful error handling with statistics tracking
Duplicate words in different forms
	
Normalized lemmatization prevents duplicates
Ambiguous articles
	
Context window analysis (3 words left/right)




📊 Statistics Tracking
The tool maintains detailed counters:

    Words accepted/rejected with reasons
    OCR success/failure rates
    Duplicate words skipped
    Expressions detected
    File processing errors

View full statistics in the interactive review table or extraction_report.json.


    AI-powered enrichment: Integrate DeepL API for automatic translations/examples
    Anki integration: Direct export to Anki flashcards with audio
    Frequency analysis: Highlight words by CEFR level (A1-C2)
    Batch processing: Watch directory for new files

Medium Term (6 Months)

    Simple GUI: Tkinter/Qt interface for non-technical users
    Browser extension: Extract vocab from web articles/PDFs online
    Pronunciation: IPA transcription + audio generation via TTS
    Spaced repetition scheduler: Built-in review planning

Long Term (12+ Months)

    Mobile app: iOS/Android companion with camera OCR
    Community database: Share/compare vocab lists with learners
    Contextual examples: Auto-generate example sentences from corpus
    Grammar hints: Identify separable verbs, adjective endings, etc.

🤝 Contributing

    Fork the repository
    Create a feature branch (git checkout -b feature/your-feature)
    Commit changes (git commit -am 'Add feature')
    Push to branch (git push origin feature/your-feature)
    Open a Pull Request

📜 License
MIT License - See LICENSE file for details



Made with ❤️ for German language learners