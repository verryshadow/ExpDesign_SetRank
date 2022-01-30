# SetRank

## Introduction

During the lecture Experiment Design at TU Vienna, we got the exercise to reproduce a paper. We chose "Entity Set Search of Scientific Literature: An Unsupervised Ranking Approach" from Jiaming Shen et al. (https://arxiv.org/pdf/1804.10877.pdf). The original implementation can be found at https://github.com/jmshen1994/SetRank. All the files were retrieved from the original repository. We made some slight changes which will be explained in the following section. The results folder containes all results from us and further analysis can be found at https://doi.org/10.5281/zenodo.5920006. 

This repo includes all the benchmark datasets, source code, evaluation toolkit, and experiment results for SetRank framework developed for entity-set-aware literature search. 

## Data 

The **./data/** folder contains two benchmark datasets used for evaluating literature search, namely _S2-CS_ and _TREC-BIO_.

## How to start

During the analysis, we used Python 3.7 and ElasticSearch 5.4.0. For executing the program, it is necessary to run ElasticSearch on your localhost. Here, you can find a good guide how to install ElasticSearch: https://www.elastic.co/guide/en/elasticsearch/reference/5.4/index.html. Our further analysis paper contains more information on how to start ElasticSearch.

Furthermore, it is necessary to install the requirements with the following command.

```
$ pip3 install -r requirements.txt
```

## Code Changes
In the code we found some reference errors like:
```
# ./code/SetRank/autoSetRank_TREC.py
# Line 413
- parser.add_argument('-output', required=False, default="../results/trec/auto-tune.run",
+ parser.add_argument('-output', required=False, default="../../results/trec/auto-tune.run",

# ./code/SetRank/autoSetRank_ESR.py
# Line 422
- parser.add_argument('-output', required=False, default="../results/s2/auto-tune.run",
+ parser.add_argument('-output', required=False, default="../../results/s2/auto-tune.run",

# ./code/SetRank/setRank_TREC.py
# Line 381
- parser.add_argument('-query', required=False, default='../../data/TREC_BIO/trec_query.json',
+ parser.add_argument('-query', required=False, default='../../data/TREC-BIO/trec_query.json',

# Line 383
- parser.add_argument('-output', required=False, default="../results/trec/setrank.run",
+ parser.add_argument('-output', required=False, default="../../results/trec/setrank.run",

# Line 385
- parser.add_argument('-kb', required=False, default="../../data/TREC_BIO/trec_entity_type.tsv")
+ parser.add_argument('-kb', required=False, default="../../data/TREC-BIO/trec_entity_type.tsv")

# ./code/SetRank/setRank_ESR.py
# Line 394
-  parser.add_argument('-output', required=False, default="../results/s2/setrank.run",
+  parser.add_argument('-output', required=False, default="../../results/s2/setrank.run",
```

## Commands
In the folder ./code/, you can find the implementations from the baseline search algorithm and for SetRank. 

### Baselines
For the baselines, it is necessary to run the create_index.py and index_data.py files, as described below. Afterwards, it is possible to run search_data.py. The respective commands are shown below. It is necessary to only use one of the values given in the brackets respectively.

```
python create\_index.py -sim ['tfidf', 'bm25', 'lm\_jm', 'lm\_dir', 'ib']
python index\_data.py -sim ['tfidf', 'bm25', 'lm\_jm', 'lm\_dir', 'ib']
python search\_data.py -sim ['tfidf', 'bm25', 'lm\_jm', 'lm\_dir', 'ib'] -mode ['both', 'word', 'entity']
```

### SetRank
For executing SetRank, it is necessary to run the create_index.py and index_data.py files. This can be done with the commands below.

```
# TREC-BIO
python create_index_TREC.py
python index_data_TREC.py

# S2-CS 
python create_index_ESR.py
python index_data_ESR.py
```

After creating the index, you can perform the search using following commands:

```
# TREC-BIO
python setRank_TREC.py -query ../../data/TREC-BIO/trec_query.json -output ../../results/trec/setRank.run

# S2-CS
python setRank_ESR.py -query ../../data/S2-CS/s2_query.json -output ../../results/s2/setRank.run
```

The results for the dataset S2-CS would here be saved to "../../results/s2/setRank.run".

## Evaluation Tool

The **./pytrec_eval/** folder includes the original evaluation toolkit [pytrec_eval](https://github.com/cvangysel/pytrec_eval) and our customized scripts for performing model evaluation.

You may first follow the instructions in **./pytrec_eval/README.md** to install this packages. For the model evaluation for the dataset S2-CS with the search algirithm SetRank, we used the following commands:

```
cd ./pytrec_eval/examples
./eval.sh ../../results/s2/setRank.run setRank
```

The commands works similarly for the dataset trec and for all baseline algorithms. 

## Significance Testing
The pytrec-eval repository contains a file for significance testing, which was reused by us. The file can be found in the following directory ttps://github.com/jmshen1994/SetRank/blob/master/pytrec_eval/examples/statistical_significance.py.

The python file can be executed using the following command. The first parameter links to the respective dataset (either in the TREC directory or S2). The other two parameters contain the path to the ".run" files of the two versions to be compared.

```
cd pytrec_eval/examples
python statistical_significance.py ../../data/S2-CS/s2.qrel ../../results/s2/baselines/bm25_both.run ../../results/s2/setRank.run --measure='ndcg_cut'
```

## Citation 

This is the citation from the offical authors:

```
@inproceedings{JiamingShen2018ess,
  title={Entity Set Search of Scientific Literature: An Unsupervised Ranking Approach},
  author={Jiaming Shen, Jinfeng Xiao, Xinwei He, Jingbo Shang, Saurabh Sinha, and Jiawei Han},
  publisher={ACM},
  booktitle={SIGIR},
  year={2018},
}
```

Furthermore, if you use the pytrec_eval toolkit, please also consider citing the original paper:

```
@inproceedings{VanGysel2018pytreceval,
  title={Pytrec\_eval: An Extremely Fast Python Interface to trec\_eval},
  author={Van Gysel, Christophe and de Rijke, Maarten},
  publisher={ACM},
  booktitle={SIGIR},
  year={2018},
}
```
