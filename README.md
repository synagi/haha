# haha

## Training Pipeline

### Configs
Each config file in */train/config* represents a planned unique finetune of the base model, as a Lora or QLora.  

This enables configs to be committed here, that then get trained and uploaded and with all their training results available.

The config's basename (name without file extension) is shared between its config here, its WandB run name, and a corresponding HuggingFace model upload.

For the pipeline to work smoothly, configs should follow these criteria:  

- The configs need to be named with 3 digits at the end, e.g. *haha-llama-3-8b-lira.yml*.
- The WandB name for the training run is set dynamically within the pipeline to the config file name, so leave it blank in input the config.  The project name should be 'haha'.
- A Lora adapter of the training is uploaded to HuggingFace, as a model with the same basename as the config.

### Dataset
Datasets should be stored in the */data* folder and reference then in the configs in a */workspace/data/* folder.  The pipeline will then include them to the training.
