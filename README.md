# English-Hindi Neural Machine Translation

This project trains an English-to-Hindi translation model using sequence-to-sequence neural networks in TensorFlow/Keras.

Training in the current notebook setup is done on a 500k-row subset from the Kaggle Hindi-English Parallel Corpus:
https://www.kaggle.com/datasets/vaibhavkumar11/hindi-english-parallel-corpus

## Project Overview

The repository contains:
- Data preparation and preprocessing for parallel English-Hindi text.
- Subword tokenization using SentencePiece.
- A GRU-based encoder-decoder model with attention.
- Training and validation workflow in Jupyter notebooks.

## Repository Structure

- `gru.ipynb`: Main notebook for preprocessing, tokenization, model training, and basic inference experiments.
- `transformer.ipynb`: Transformer-based experimentation notebook.
- `hindi_english_parallel.csv`: Parallel dataset used in notebook workflows.
- `requirements.txt`: Python dependencies.
- `eng_hin.model` and `eng_hin.vocab`: SentencePiece tokenizer artifacts.
- `corpus.txt`: Corpus used for tokenizer training.
- `models/` and `data/`: Additional model/data folders.

## Requirements

- Python 3.10+ recommended.
- pip
- (Optional) virtual environment support (`venv` or Conda).

## Setup

1. Clone the repository:

```bash
git clone https://github.com/zatiyab/eng-hin-translator.git
cd eng-hin-translator
```

2. Create and activate a virtual environment:

Windows (PowerShell):
```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
```

Windows (Git Bash):
```bash
python -m venv .venv
source .venv/Scripts/activate
```

3. Install dependencies:

```bash
pip install -r requirements.txt
```

4. Launch Jupyter:

```bash
jupyter notebook
```

Then open `gru.ipynb`.

## Training Workflow (GRU Notebook)

Typical flow in `gru.ipynb`:
1. Load and clean parallel data.
2. Build corpus and train SentencePiece tokenizer.
3. Convert English/Hindi text into token ID sequences.
4. Pad sequences to fixed lengths.
5. Train GRU encoder-decoder with multi-head attention.
6. Save model artifact.

## Model Architecture (GRU + Attention)

The model in `gru.ipynb` is a sequence-to-sequence architecture with attention:

- Tokenization: Shared SentencePiece vocabulary (`vocab_size=16000`) for both English and Hindi.
- Encoder: Embedding layer (`256`) followed by a bidirectional GRU (`512` units each direction, `return_sequences=True`).
- Decoder: Embedding layer (`256`) followed by a GRU (`512` units, `return_sequences=True`).
- Attention: Multi-head cross-attention (`8` heads, `key_dim=64`) where decoder states attend over encoder outputs.
- Fusion + Output: Decoder GRU output is concatenated with attention output, passed through a `Dense(512, relu)`, then projected to vocabulary logits with a final softmax layer.

Training uses teacher forcing with shifted Hindi sequences:
- `decoder_input = hindi_padded[:, :-1]`
- `decoder_target = hindi_padded[:, 1:]`

Padding tokens are masked using sample weights so PAD positions do not dominate the loss.

## Generated Artifacts

During notebook execution, you may generate:
- `corpus.txt`
- `eng_hin.model`
- `eng_hin.vocab`
- `gru_model.keras` (or similarly named Keras model file)

## Notes

- The notebook contains a mix of older tokenizer code and SentencePiece-based code. If you see errors involving undefined variables like `english_tokenizer` or `hindi_tokenizer`, keep the pipeline consistent by using only one tokenization approach.
- For faster experiments, reduce dataset slice size and epochs before full training.

## Future Improvements

- Add a clean inference pipeline (greedy/beam decoding) using SentencePiece end to end.
- Add reproducible training scripts outside notebooks.

## License

No license file is currently included. Add a `LICENSE` file to define reuse terms.
