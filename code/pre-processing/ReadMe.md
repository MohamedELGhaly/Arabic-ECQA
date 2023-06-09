In this section of the repository, you will find instructions on how to preprocess the data for fine-tuning the Aragpt2 model.

## Data Preprocessing

Ensure that you have the required data files from the dataset repository, and paste them into the data directory located in the current root.

```
Arabic-OMCS.json
Arabic-ECQA.csv
Arabic-ECQA_data_test.csv
Arabic-ECQA_data_train.csv
Arabic-ECQA_data_val.csv
```

Then run the following commands to generate the processed data for training and running inference on the models.



To conduct experiments using our train, validation, and test splits, then run the following commands -

```bash
cd data
python3 E2_data_generator_author_split.py
```

If you want to use a random 70-10-20 train-dev-test split from the total dataset, then run the following commands -

```bash
cd data
python3 E2_data_generator.py
```