# Prompt Extraction Attack

## Setup

Python 3.10 + `pip install -r requirements.txt`

```
attacks/
├── real-services/          <- prompt extractions from Bard, Bing Chat, ChatGPT and Claude
├── 5-gram-attacks.json     <- extraction attack against a 5-gram output filtering defense
├── attacks.json            <- handwritten attack queries
├── generated.json          <- attack queries generated by GPT-4
└── selected.json           <- 15 most effective attack queries
data/
├── dev.jsonl               <- validation data
├── sharegpt-test.jsonl     <- sharegpt test set
└── unnatural-test.jsonl    <- unnatural instructions test set
src/
├── common.py                           <- common utils
├── evaluate-extraction.py              <- evaluate extraction outputs
├── finetune-similarity-predictor.py    <- fine-tune a DeBERTa model for confidence estimation
├── gpt-x-prompt-extraction.py          <- GPT-3.5 & GPT-4 extraction
├── guess-model/                        <- identify LLMs with generations                        
├── hf-prompt-extraction.py             <- HuggingFace LLMs extraction
├── llama-chat-prompt-extraction.py     <- Llama-2-chat extraction
```

## LLM system prompts

All extracted system prompts for Bing Chat, Bard and ChatGPT can be found in `attacks/real-services`.

## Prompt extraction 

To reproduce our prompt extraction results on GPT-4 & `awesome-chatgpt-prompts`:
```bash
python src/gpt-x-prompt-extraction.py \
 --model gpt-4 \
 --data awesome \
 --attack attacks/generated.json
```

Notes:
- Adding the argument `--defense "5-gram"` enables the 5-gram defense.
- Specify source of prompt with `--data`. Valid choices are `dev`, `awesome`, `sharegpt` and `unnatural`.
- See `src/hf-prompt-extraction.py` and `src/llama-chat-prompt-extraction.py` for HF/Llama-2-chat prompt extraction.

## Fine-tuned prompt leakage model

[Model weights are on Hugging Face](https://huggingface.co/yimingzhang/deberta-v3-large-prompt-leakage).
See `src/evaluate-extraction.py` on how to use the model to predict prompt leakage.


## LICENSE

MIT
