# Haha

Haha is an LLM finetune for generating punchlines to joke setups, as an experiment in creative writing.

## Training Pipeline

The Synagi training pipeline uses [Axolotl](https://github.com/OpenAccess-AI-Collective/axolotl) and automatically deploys HuggingFace models with corresponding Weights & Biases data, using the configs and datasets merged to the main branch of this repo.
- [Model YAML Configs](https://github.com/synagi/haha/tree/main/train/config): Each config represent a set of hyperparameters for the Haha fintune
- [Finetune Datasets](https://github.com/synagi/haha/tree/main/train/data): Data used for finetuning in Alpaca format
- [HuggingFace models](https://huggingface.co/synagi): Trained models, using ChatML prompt format
- [Weights & Biases data](https://wandb.ai/freegheist/haha?nw=nwuserfreegheist): Training results

### Configs
Each config file in */train/config* represents a planned unique finetune of the base model, as a Lora or QLora.  

This enables configs to be committed here, that then get trained and uploaded and with all their training results available.

The config's basename (name without file extension) is shared between its config here, its WandB run name, and a corresponding HuggingFace model upload.

#### Config Format  

- The configs need to be named consecutively with 3 digits at the end, e.g. *haha-llama-3-8b-lora-001.yml*.
- The WandB name for the training run is set dynamically within the pipeline to the config file name, so leave it blank in input the config.  The project name should be 'haha'.
- A Lora adapter of the training is uploaded to HuggingFace, as a model with the same basename as the config.

#### Example
```
base_model: meta-llama/Meta-Llama-3-8B
...

chat_template: chatml
default_system_message: You are a creative and hilarious comedy writer that loves to craft jokes.

datasets:
  - path: /workspace/data/haha-v1.jsonl
    type:
      system_prompt: "system\nYou are a creative and hilarious comedy writer that loves to craft jokes.<|im_end|>\n"
      field_system: system
      field_instruction: instruction
      field_output: output
      format: "<|im_start|>user\n{instruction}<|im_end|>\n<|im_start|>assistant\n"
      no_input_format: "<|im_start|>user\n{instruction}<|im_end|>\n<|im_start|>assistant\n"

dataset_prepared_path: ./last_run_prepared
val_set_size: 0.01
output_dir: /workspace/output
...
```

### Datasets
Datasets should be stored in the */data* folder, and then referenced within the configs in a */workspace/data/* folder.  The pipeline will use them for the training.

#### Config Format
The dataset formats should be Alpaca, and are trained using the ChatML prompt format.

#### Example
```
{"instruction": "In a survey of 35 cities, Los Angeles ranked second to last in \"intelligence.\"", "output": "Residents of LA were outraged after the report was slowly explained to them.", "input": ""}
```
