Model Name
English→Hindi NMT

Architecture
BiGRU Encoder
GRU Decoder
MultiHeadAttention

Vocabulary
SentencePiece Unigram
16k vocab

Parameters
~22M

Training Data
~500k sentence pairs

Evaluation
BLEU ≈ 6.3

Built from scratch without pretrained translation models.

Features:
- SentencePiece tokenization
- Bidirectional GRU encoder
- GRU decoder
- Multi-head cross-attention
- Teacher forcing training
