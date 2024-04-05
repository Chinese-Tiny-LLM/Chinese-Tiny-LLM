# Chinese-Tiny-LLM

[**🤗 MAP-CC**](https://huggingface.co/datasets/m-a-p/MAP-CC) | [**🤗 CHC-Bench**](https://huggingface.co/datasets/m-a-p/CHC_Bench) | [**🤗 CT-LLM**](https://huggingface.co/collections/m-a-p/chinese-tiny-llm-660d0133dff6856f94ce0fc6) | [**📖 arXiv**]() | [**GitHub**](https://github.com/XinrunDu/Chinese-Tiny-LLM)

This repository contains our suite of procedures for cleaning Chinese web corpora and pre-training code.

## Overview

We introduce CT-LLM, a 2B parameter language model, marking a shift towards focusing on the Chinese language for LLM development. Starting from scratch, CT-LLM primarily uses Chinese data from a 1,200 billion token corpus, including 800 billion Chinese, 300 billion English, and 100 billion code tokens. This mix enhances its Chinese processing abilities, further improved by alignment techniques. CT-LLM shows excellent performance in Chinese language tasks on the CHC-Bench and is also adept in English through SFT. This approach challenges the norm of relying on English corpora for LLM training, expanding training methodologies. By open-sourcing CT-LLM's training process, including data processing and the Massive Appropriate Pretraining Chinese Corpus (MAP-CC), and introducing the Chinese Hard Case Benchmark (CHC-Bench), we encourage further research and innovation, aiming for more inclusive and adaptable language models.

- **MAP-CC** An open-source Chinese pretraining dataset with a scale of 800 billion tokens, along with a detailed suite of procedures for cleaning Chinese web corpora, offering the NLP community high-quality Chinese pretraining data and an effective methodology for data preparation. 

- **CHC-Bench** A well-chosen multidisciplinary Chinese hard cases instruction understanding and following benchmark. 

- **CT-LLM** The first Chinese-centric large language model, both pre-training and fine-tuned primarily on Chinese corpora, offers significant insights into potential biases, Chinese language ability, and multilingual adaptability.

## Filter
You first need to download fasttext model in the filter directory. The download [link](https://dl.fbaipublicfiles.com/fasttext/supervised-models/lid.176.bin). The command example is 
```
wget -P https://dl.fbaipublicfiles.com/fasttext/supervised-models/lid.176.bin
```

Once you download the model bin of fasttext, then you use command like follows to filter text.
```
python3 filter/filter.py --input_data input_dir --output_dir output_dir --success_dir success_dir --log_dir log_dir --worker num_worker
```
where input_dir is the direcotry containing jsonl files, output_dir is the directory to store the filtered jsonl files, success_dir is dirtory to save files about whether files are processed successfully, log_dir is the directory to save logs and num_worker is the total number of process to execute the job in parallel. 

## Deduplication

### Full Text Deduplication

Here is an exmaple command to run full text deduplication
```
python3 deduplication/full_text_dedup/full_text_dedup.py --input_dir input_dir --output_dir output_dir --content_field_name content_field_name
```
where input_dir is the directory to story the jsonl files, output_dir is the dirtory to save the deduplicated jsonl file and content_field_name is the field name of content in jsonl file.

To reduce the total processing time, you can partition jsonl files to multiple partitions, and then run multiple job for each partition in parallel.


### Minhash LSH
#### Generate Minhash
Here is an exmaple command to run Generate Minhash
```
python3 deduplication/minhash_lsh/generate_minhash.py --input_dir input_dir --output_dir output_dir --workers num_workers --content_field_name content_field_name
```
where input_dir is dirtory to save jsonl files, output_dir is the directory to save minhash value, num_worker is  the total number of processes executing the job and content_field_name is the field name of content in jsonl file.

#### Gereate Duplicate Pairs
Here is an example command to run  Gereate Duplicate Pairs
```
python3 deduplication/minhash_lsh/generate_dup_pairs.py --input_dir input_dir --output_dir output_dir 
```
where input_dir is the directory to storay the minhash values, which is the same with the output_dir in Generate Minhash. Output_dir is the directory to save the duplicate paris files.

#### Genereate Connected Componenets
Here is an example command to run Genereate Connected Componenets
```
python3 deduplication/minhash_lsh/generate_connected_components.py --input_dir input_dir --output_file output_file --num_workers num_workers
```
where input_dir is the directory containing the duplicate lines file which is the same as the output_dir in Generate Duplicate Pairs. Output_file is file path to save information about connected components. Num_workers is the total number of processes executing the job.

#### Generate Duplicated Line ID for Each File
Here is an example command to run Genereate Connected Componenets
```
python3 deduplication/minhash_lsh/generate_dup_line_id_for_each_file.py --input_file input_file --output_dir output_dir
```
where input_file is the file containing nformation about connected components which is the same as output_file in Genereate Connected Componenets. Output_dir is the directory to save duplicate line id information for each file.

#### Remove Duplicates
Here is an example command to run Remove Duplicates
```
python3 deduplication/minhash_lsh/remove_dup.py --input_dir input_dir --output_dir output_dir --dup_line_id_dir dup_line_id_dir --worker num_worker
```
where input_dir is the directory containing the original jsonl files which is the same as the input_dir in Generate Minhash. Output_dir is the directory to save the dedupliacted jsonl files. Dup_line_id_dir is the directory containing duplicate line ids of all jsonl files which is the same as output_dir in Generate Duplicated Line ID for Each File.

### Similar Line Deduplication
Here is an example command to run Remove Duplicates
```
python3 deduplication/simlar_line_dedup/similar_line_dedup.py --input_dir input_dir --output_dir output_dir --wokers num_worker --content_field_name content_field_name
```
where input_dir is the directory containing the jsonl files, output_dir is the direcotry to save deduplicated jsonl files, num_worker is the total number of processes executing the job and content_field_name is the field name of content in jsonl file. 


## Pre-training

Coming soon...


