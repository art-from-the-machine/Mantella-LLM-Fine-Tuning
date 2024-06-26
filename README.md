# Mantella LLM Fine-Tuning
This repository is intended to store and document the processes used to extract LLM fine-tuning datasets for the creation of custom-tailored LLMs for [Mantella](https://art-from-the-machine.github.io/Mantella/).

## Skyrim Dialogue Dataset
The Skyrim dialogue dataset is an Alpaca-style dataset containing 8,800+ input / output examples of player <-> NPC interactions.

Example:
```json
{
    "instruction": "Generate dialogue in the style of Skyrim.",
    "input": "Where can I find fire salts?",
    "output": "A flame atronach's body might provide fire salt. They're dangerous creatures that can be summoned by wizards. Of course, it would be much easier to check with an alchemist. They occasionally have them for sale."
}
```

This dataset can be generated by following the steps in the `skyrim_dataset.ipynb` notebook. [Lazy Voice Finder](https://www.nexusmods.com/skyrim/mods/82482) is required to first extract the raw Skyrim dataset. You need to own a copy of Skyrim in order to access the raw data.

This dataset has been used to fine-tune Meta's Llama 3 8B Instruct model. You can download the GGUFs for this fine-tuned model from [Hugging Face](https://huggingface.co/art-from-the-machine/Mantella-Skyrim-Llama-3-8B-GGUF).

## Future Work
### Expand dataset with synthetic data
The existing Skyrim dialogue dataset could be expanded with synthetic question / answer examples generated from in-game texts: [https://github.com/jd7h/sentiment-lexicon-skyrim/blob/master/data/imperial_library.json](https://github.com/jd7h/sentiment-lexicon-skyrim/blob/master/data/imperial_library.json)

### Fallout 4 dataset
The same process to extract dialogue from Skyrim using Lazy Voice Finder could also be applied to Fallout 4.

### Prepend NPC name to assistant response
It may be possible to extract the name of the NPC delivering a given response from the File Name & Voice Type columns of `LazyVoiceFinder_export.csv`. Starting a response with the NPC's name would benefit a fine-tuned model as this information is processed by Mantella in multi-NPC conversations.

Example response: "Hulda: Return anytime. You're quite welcome here."

### Change instruction wording
Every instruction in the final `skyrim_alpaca_style_dataset.json` dataset has the same "Generate dialogue in the style of Skyrim." text. Experimenting with different wordings of this instruction has not been explored, and neither has varying the instruction depending on the input / output variables.

### Prepend NPC actions to assistant response
Mantella can read three NPC actions; `Follow`, `Offended`, and `Forgiven`. The final `skyrim_alpaca_style_dataset.json` dataset could be edited to contain these actions in related NPC responses. For example, if the player input is "Follow me. I need your help.", then the `Follow` action could be prepended to the NPC's response in cases where the NPC agrees.

### Prepend emotion to response
Each response in `LazyVoiceFinder_export.csv` is assigned an emotion by the Emotion column. While Mantella does not currently support emotion tags, if they were to be processed in the future the data is freely available in this dataset for finetuning.

### Further Cleaning
While steps have been taken to clean the `skyrim_alpaca_style_dataset.json` dataset in `skyrim_dataset.ipynb`, there may still be poor training examples in the dataset that need to be edited / removed.
