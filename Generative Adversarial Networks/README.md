# Generative Adversarial Networks for Bulk RNAseq

## Install requirements
install `python >= 3.5`

```
pip install numpy pandas sklearn keras tensorflow-gpu matplotlib pillow argparse
```

## Data collection and pre-processing

1. create query to pull down all available read counts information; 

```
gsutil cat gs://ncbi_sra_rnaseq/genecounts/*.genecounts > compiled_genecounts.csv
```

2. concatenate samples into one single file;

3. associate metadata with project IDs through custom script;

4. transform read counts to narrow them down to the range [0, 1]

## GAN pipeline

input: normalized read counts for experimental and control samples

### Training
```
python src/exp_gan.py train --input_file data/test.csv --output_dir=train --n_epoch=500 --batch_size=10 --training_ratio=1
```

### Generating samples
```
python src/exp_gan.py generate --model_file train/models/generator_epoch_499.h5 --output_file output/test.csv --n_samples 10
```

## docker
### Build 
```
docker build -t exp_gan:0.1.0 .
```
### Run
```
docker run --rm exp_gan:0.1.0
```