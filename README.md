# Classificazione decoder-only di tweet politici italiani

Task di classificazione binaria: `0 = NON_HATE`, `1 = HATE`.

E stato realizzato il finetuning dei modelli encoder-only AlBERTo con diverse strategie di pre-processing al fine di selezionare la pipeline più adatta al task di classificazione. La pipeline selezionata è disponibile nella cartella principale, mentre le altre configurazioni sono raccolte nella cartella altri esperimenti-encoder.

# Attenzione

Adattare il path dei dataset al proprio ambiente di esecuzione.

# Encoder pipeline(s)

Modello utilizzato
- AlBERTo: `m-polignano-uniba/bert_uncased_L-12_H-768_A-12_italian_alb3rt0` disponibile su hugging face

# Modello e strategie di pre-processing valutate

| Modello e configurazione | Validation macro-F1 | Test macro-F1 |
| --- | --- | --- |
| AlBERTo, raw text | 0.9178 | 0.9107 |
| AlBERTo, removal of [url] and @user mentions + emoji tokens | 0.9153 | 0.9143 |
| AlBERTo, preprocessing + enriched hashtag/PER/ORG inputs | 0.9258 | 0.9106 |
| AlBERTo, preprocessing + enriched hashtag/PER/ORG inputs + followers + replies | 0.9156 | 0.915 |


## Modello e pipeline selezionata

Sulla base della performance (macro-F1) sul set di validazione: AlBERTo, preprocessing + enriched hashtag/PER/ORG inputs.


# Decoder pipeline(s)


Sono state valutate due approcci tramite modelli decoder only: tramite inferenza (in context learning zero-shot e few-shot) e fine-tuning supervisionato. La pipeline selezionata è disponibile nella cartella principale, mentre le altre configurazioni sono raccolte nella cartella altri esperimenti-decoder.

1. **Dati e preprocessing.** Rimozione di URL, placeholder `[URL]` e menzioni `@user`, seguita dalla normalizzazione degli spazi. 

2. **Inferenza** Modelli utilizzati: `meta-llama/Llama-3.2-3B-Instruct` e `sapienzanlp/Minerva-7B-instruct-v1.0` quantizzati in 4 bit. Il prompt italiano specifica la definizione del task e richiede HATE o NON_HATE. Few-shot: otto esempi training, quattro per classe, selezionati con seed 42. 

3. **Fine-tuning.** di `walid-iguider/Minerva-3B-Instruct-v1.0` tramite Unsloth  e  LoRA.


# Esperienze realizzate

| Modello e configurazione | Validation macro-F1 | Test macro-F1 |
| --- | --- | --- |
| Llama-3.2-3B-Instruct, zero-shot | 0.7154 | 0.7064 |
| Llama-3.2-3B-Instruct, few-shot | 0.6325 | 0.6555 |
| Minerva-7B-Instruct, zero-shot | 0.2926 | 0.3353 |
| Minerva-7B-Instruct, few-shot | 0.6407 | 0.6551 |
| Minerva-3B-Instruct fine-tuned | 0.8994 | 0.9250 |

### Modello selezionato

Minerva-3B-Instruct fine-tuned.


# Esecuzione e controlli

Notebook eseguiti su Google Colab. Installazioni necessarie indicate sul notebook.



