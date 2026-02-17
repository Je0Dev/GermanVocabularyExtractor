# 🇩🇪 German Vocabulary Extractor

A production-ready tool for extracting German vocabulary from learning materials (PDF, DOCX, PPTX) with linguistic analysis, OCR support, and intelligent deduplication.

## ✨ Key Features

- **Multi-format support**: PDF (text + image OCR), DOCX, PPTX
- **Linguistic enrichment**:
  - Articles (`der/die/das`) detection from context
  - Plural form generation for nouns
  - Case usage detection (Akkusativ/Dativ/Genitiv) via prepositions
- **Smart proper noun detection**: Distinguishes common German nouns from names using context analysis
- **Expression extraction**: Multi-word phrases and fixed expressions
- **Strict deduplication**: Normalized lemmatization prevents duplicates
- **Uncertainty handling**: Borderline words stored separately for review
- **Large text optimized**: Streaming processing for books/essays
- **Interactive review**: Summary table before final output
- **Dual output formats**: Clean `.txt` + structured `.csv` with metadata
- **Language detection**: Skips non-German content automatically (bypassable with `--force`)

## 🚀 Quick Start

### Prerequisites

#### Ubuntu/Debian
```bash
# Install system dependencies
sudo apt-get install tesseract-ocr tesseract-ocr-deu poppler-utils

# Install Python dependencies
pip install pdfplumber PyPDF2 python-docx python-pptx Pillow pytesseract pandas tqdm langdetect

# Optional: For full linguistic features (requires Python 3.11-3.12)
pip install spacy pattern
python -m spacy download de_core_news_lg
```

#### Arch Linux ⭐
```bash
# Install system dependencies
sudo pacman -S --needed tesseract tesseract-data-deu git base-devel

# Create virtual environment
python -m venv venv
source venv/bin/activate

# Install Python dependencies
pip install --upgrade pip
pip install PyPDF2 pdfplumber python-docx python-pptx Pillow pytesseract pandas tqdm langdetect

# Optional: For full linguistic features (Python 3.11-3.12 recommended)
pip install spacy pattern
python -m spacy download de_core_news_lg
```

#### macOS
```bash
# Install system dependencies
brew install tesseract poppler

# Install Python dependencies
pip install pdfplumber PyPDF2 python-docx python-pptx Pillow pytesseract pandas tqdm langdetect
```

### Installation

```bash
# Clone the repository
git clone https://github.com/yourusername/german-vocab-extractor.git
cd german-vocab-extractor

# Create virtual environment (recommended)
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt
```

## 📖 Usage

### Basic Usage

```bash
# Extract vocabulary from a single file
python german_vok_extr.py book.pdf

# Extract from multiple files
python german_vok_extr.py book.pdf exercises.docx presentation.pptx

# Force processing (bypass language detection)
python german_vok_extr.py trans.pdf --force

# Custom minimum word length
python german_vok_extr.py --min-length 4 german_text.pdf

# Specify output directory
python german_vok_extr.py --output-dir my_vocab german_book.pdf

# Disable OCR (faster for text-only PDFs)
python german_vok_extr.py --no-ocr document.pdf

# Verbose logging
python german_vok_extr.py --verbose document.pdf
```

### Command Line Options

| Option | Description | Default |
|--------|-------------|---------|
| `files` | Input files (PDF, DOCX, PPTX) | Required |
| `--min-length` | Minimum word length to include | `3` |
| `--output-dir` | Output directory | `output` |
| `--no-ocr` | Disable OCR for image extraction | `False` |
| `--force` | Force processing (bypass German language check) | `False` |
| `--verbose` | Enable debug logging | `False` |

## 📁 Output Files Generated

```
output/
├── vocabulary_20260213_143022.txt      # Clean line-by-line format
├── vocabulary_20260213_143022.csv      # Structured with metadata
├── uncertain_words_20260213_143022.txt # Borderline words for review
├── uncertain_words_20260213_143022.csv
├── expressions_20260213_143022.txt     # Multi-word phrases
└── extraction_report_20260213_143022.json # Processing statistics
```

## 📝 Sample Output

### vocabulary_*.txt
```
Haus (das) Pl: Häuser
gehen Pl: gehen [Akkusativ]
Buch (das) Pl: Bücher
geben Pl: geben [Dativ, Akkusativ]
Freundschaft (die) Pl: Freundschaften
schnell Pl: schnell
mitbringen Pl: mitbringen [Akkusativ]
```

### vocabulary_*.csv
```csv
original,lemma,article,plural,case_usage,pos,source,context
Haus,Haus,das,Häuser,,NOUN,book.pdf,im Haus mit der...
gehen,gehen,,gehen,,VERB,book.pdf,wir gehen nach...
```

### uncertain_words_*.txt
```
Spiros | Reason: possible_proper_noun | Context: zur Prüfung Goethe-Zertifikat Spiros Koukidis...
Koukidis | Reason: possible_proper_noun | Context: Prüfung Goethe-Zertifikat Spiros Koukidis...
```

## ⚙️ Advanced Configuration

### Script Configuration

Edit the script to customize:
- **Stop word lists**: Add/remove common words to exclude
- **Case preposition mappings**: Customize preposition-case relationships
- **Expression detection patterns**: Add custom multi-word expressions
- **Common German nouns list**: Expand the `COMMON_GERMAN_NOUNS` set for better proper noun detection
- **Confidence thresholds**: Adjust inclusion criteria

### Python Version Compatibility

| Python Version | Recommended Version | Notes |
|----------------|--------------------|-------|
| 3.11-3.12 | ✅ **Full version** | Supports spaCy + pattern for advanced linguistics |
| 3.13+ | ✅ **Lite version** | Use `german_vok_extr.py` without spaCy |
| 3.14 | ⚠️ **Lite version only** | spaCy not yet fully compatible |

## 🧪 Edge Cases Handled

| Scenario | Handling |
|----------|----------|
| Scanned PDFs with images | Tesseract OCR with German language pack |
| Mixed-language documents | Language detection skips non-German content |
| German nouns (capitalized) | Context analysis + `COMMON_GERMAN_NOUNS` list |
| Proper names | Detected via article absence + frequency analysis |
| Compound German words | Regex tokenization with umlaut support |
| Umlauts (äöüß) | Full Unicode support in all outputs |
| Large files (>100MB) | Streaming processing with progress bars |
| Corrupted files | Graceful error handling with statistics tracking |
| Duplicate words | Normalized lemmatization prevents duplicates |
| Ambiguous articles | Context window analysis (3 words left/right) |
| OCR failures | Fallback to PyPDF2 text extraction |

## 📊 Statistics Tracking

The tool maintains detailed counters visible in the interactive review:

- **Words accepted/rejected** with reasons
- **OCR success/failure** rates
- **Duplicate words** skipped
- **Expressions** detected
- **File processing** errors
- **Pages processed** (text vs. images)

View full statistics in the interactive review table or `extraction_report.json`.

## 🔧 Troubleshooting

### Tesseract OCR Errors
```bash
# Error: Failed loading language 'deu'
# Solution: Install German language pack

# Ubuntu/Debian
sudo apt-get install tesseract-ocr-deu

# Arch Linux
sudo pacman -S tesseract-data-deu

# Verify installation
tesseract --list-langs
```

### Python 3.14 Compatibility Issues
```bash
# If spaCy fails to install, use the lite version
# The lite version works without spaCy/pattern

# Install without spaCy
pip install PyPDF2 pdfplumber python-docx python-pptx Pillow pytesseract pandas tqdm langdetect

# Run with --force flag if language detection fails
python german_vok_extr.py document.pdf --force
```

### Virtual Environment Activation Issues
```bash
# Bash/Zsh
source venv/bin/activate

# Windows CMD
venv\Scripts\activate

# Windows PowerShell
venv\Scripts\Activate.ps1

# Alternative: Run without activation
./venv/bin/python german_vok_extr.py document.pdf
```

### CSV Fieldname Errors
```bash
# If you get "dict contains fields not in fieldnames" error
# This has been fixed in the latest version
# Make sure you're using the updated script
```

## 🛣️ Roadmap

### Short Term (1-3 Months)
- [x] Python 3.14 compatibility (lite version)
- [x] Improved proper noun detection
- [x] Arch Linux installation guide
- [ ] AI-powered enrichment: Integrate DeepL API for automatic translations/examples
- [ ] Anki integration: Direct export to Anki flashcards with audio
- [ ] Frequency analysis: Highlight words by CEFR level (A1-C2)

### Medium Term (6 Months)
- [ ] Simple GUI: Tkinter/Qt interface for non-technical users
- [ ] Browser extension: Extract vocab from web articles/PDFs online
- [ ] Pronunciation: IPA transcription + audio generation via TTS
- [ ] Spaced repetition scheduler: Built-in review planning

### Long Term (12+ Months)
- [ ] Mobile app: iOS/Android companion with camera OCR
- [ ] Community database: Share/compare vocab lists with learners
- [ ] Contextual examples: Auto-generate example sentences from corpus
- [ ] Grammar hints: Identify separable verbs, adjective endings, etc.

## 📚 Dependencies

### Core Dependencies
- `PyPDF2` - PDF text extraction fallback
- `pdfplumber` - Primary PDF text/image extraction
- `python-docx` - DOCX file support
- `python-pptx` - PPTX file support
- `Pillow` - Image processing
- `pytesseract` - OCR engine wrapper
- `pandas` - Data manipulation and CSV export
- `tqdm` - Progress bars
- `langdetect` - Language detection

### Optional Dependencies (Full Version)
- `spacy` - Advanced NLP (lemmatization, POS tagging)
- `pattern` - German linguistic features (plural generation)

### System Dependencies
- `tesseract-ocr` - OCR engine
- `tesseract-ocr-deu` - German language data
- `poppler-utils` - PDF utilities (Linux)

## 🤝 Contributing

Contributions are welcome! Please follow these steps:

1. **Fork** the repository
2. **Create** a feature branch (`git checkout -b feature/your-feature`)
3. **Commit** changes (`git commit -am 'Add feature'`)
4. **Push** to branch (`git push origin feature/your-feature`)
5. **Open** a Pull Request

### Development Setup
```bash
# Clone your fork
git clone https://github.com/yourusername/german-vocab-extractor.git
cd german-vocab-extractor

# Create virtual environment
python -m venv venv
source venv/bin/activate

# Install in development mode
pip install -e .

# Run tests
python -m pytest tests/
```

## 📜 License

MIT License - See [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- **Tesseract OCR** - Open-source OCR engine
- **spaCy** - Industrial-strength NLP library
- **pdfplumber** - PDF text extraction library
- German language learners community for feedback and testing

## 📞 Support

- **Issues**: [GitHub Issues](https://github.com/yourusername/german-vocab-extractor/issues)
- **Discussions**: [GitHub Discussions](https://github.com/yourusername/german-vocab-extractor/discussions)
- **Email**: your.email@example.com

---

**Made with ❤️ for German language learners**

*Last updated: February 2026*