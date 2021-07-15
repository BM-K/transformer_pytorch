# Usage

# 라이브러리 설치
```bash
pip install msgpack==1.0.2
pip install msgpack-numpy==0.4.7.1
pip install spacy==2.3.5
pip install torchtext==0.4
```

## WMT'16 Multimodal Translation: de-en

An example of training for the WMT'16 Multimodal Translation task (http://www.statmt.org/wmt16/multimodal-task.html).

### 0) Download the spacy language model.
```bash
# conda install -c conda-forge spacy 
python -m spacy download en
python -m spacy download de
```

### 1) Preprocess the data with torchtext and spacy.
```bash
python preprocess.py -lang_src de -lang_trg en -share_vocab -save_data m30k_deen_shr.pkl
```

### 2) Train the model

```bash
python train.py -data_pkl m30k_deen_shr.pkl -embs_share_weight -proj_share_weight -label_smoothing -output_dir output -b 256 -warmup 128000 -epoch 400
```

### 3) Test the model

```bash
python translate.py -data_pkl m30k_deen_shr.pkl -model ./output/model.chkpt -output prediction.txt
```

### 4) calculate bleu
```bash
python bleu.py --reference ./.data/multi30k/test2016.en --candidate prediction.txt
```

# Performance

## Normal

|de -> en|

layer 6, head 8
- (training) ppl : 6.85121, acu : 85.685 %
- (validation) ppl : 13.92095, acu: 62.202 %
- (test) bleu : 0.37815

|en -> de|

layer 6, head 8
- (training) ppl : 6.66036, acu : 86.251 %
- (validation) ppl : 14.95462, acu: 59.245 %
- (test) bleu : 0.20529

----------------------------------------------
## Scaling

|de -> en|

layer 6, head 8
- (training) ppl : 6.85121, acu : 85.685 %
- (validation) ppl : 13.92095, acu: 62.202 %
- (test) bleu : 0.37815

|en -> de|

layer 6, head 8
- (training) ppl : 6.66036, acu : 86.251 %
- (validation) ppl : 14.95462, acu: 59.245 %
- (test) bleu : 0.20529
