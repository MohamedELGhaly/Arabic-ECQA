# Generation Models

In this repository section, you will find the guidelines for preprocessing the data to prepare it for fine-tuning the AraGPT2 model.

## Data Preprocessing

Begin by executing the data pre-processing scripts located in the[pre-processing section](https://github.com/ShouryaAggarwal/Explanations-for-CommonSenseQA/tree/master/retrieval), and paste all the obtained json files in the data directory of the current root.

```bash
cp ../pre-processing/data/*.json ./data/
```
Then run the following commands to generate the processed data for training and running inference on the models.

```bash
cd data
python3 preprocess.py
python3 preprocess_E2.py
python3 preprocess_free_flow.py
python3 preprocess_props_freeflow.py
```

## Ara-AX

### Training

```bash
cd language-modeling
python3 run_language_modeling_queries.py --output_dir=<path to save intermediate model> --model_type=gpt2 --model_name_or_path=aubmindlab/aragpt2-base --do_train --train_data_file=../data/train.txt --do_eval --eval_data_file=../data/valid.txt --per_device_train_batch_size=20 --per_device_eval_batch_size=20 --line_by_line --evaluation_strategy=epoch --learning_rate 5e-5 --num_train_epochs=5 --overwrite_output_dir --save_steps 100000 --block_size 50 --prediction_loss_only
python3 run_language_modeling_queries.py --output_dir=<path to save model> --model_type=gpt2 --model_name_or_path=<path to saved intermediate model> --do_train --train_data_file=../data/E2_GPT_train.txt --do_eval --eval_data_file=../data/E2_GPT_valid.txt --per_device_train_batch_size=20 --per_device_eval_batch_size=20 --line_by_line --evaluation_strategy=epoch --learning_rate 5e-5 --num_train_epochs=5 --overwrite_output_dir --save_steps 100000 --block_size 250 --prediction_loss_only
```

### Inferencing

```bash
cd text-generation
python3 run_GPT2.py -test_file ../data/E2_GPT_test.json -pretrained_model <path to saved model> -max_length 150 -model_type gpt2 -output_file gpt2_props_output.json
python3 GPT_json_output_parsing.py -input gpt2_props_output.json -output gpt2_props_output.json
```

## Ara-FX

### Training

```bash
cd language-modeling
python3 run_language_modeling_queries.py --output_dir=<path to save model> --model_type=gpt2 --model_name_or_path=aubmindlab/aragpt2-base --do_train --train_data_file=../data/E2_GPT_freeflow_train.txt --do_eval --eval_data_file=../data/E2_GPT_freeflow_valid.txt --per_device_train_batch_size=20 --per_device_eval_batch_size=20 --line_by_line --evaluation_strategy=epoch --learning_rate 5e-5 --num_train_epochs=10 --overwrite_output_dir --save_steps 100000 --block_size 250 --prediction_loss_only
```

### Inferencing

```bash
cd text-generation
python3 run_GPT2_freeflow.py -test_file ../data/E2_GPT_freeflow_test.json -pretrained_model <path to saved model> -max_length 250 -model_type gpt2 -output_file gpt2_raw_freeflow_output.json
```