# Survey

This repository contains all code used to generate the statistics and plots in the paper 
**Survey on English Entity Linking on Wikidata**. \
Additionally, the generated statistics and plots are available. 


Also, the code does not necessarily run out of the box. Sometimes, small adjustments to the code have to be made like 
setting a variable or adding a function call.
In the following, all relevant scripts are shortly described and necessary adjustments pointed out.

## Installation

Run `pip install -r  requirements.txt`.

## Dataset evaluation

### Getting the datasets
To generate the statistics again, one has to download the necessary datasets. 
The links to the datasets can be found in folder [datasets](datasets). 
Both files [dataset_links.json](datasets/dataset_links.json) and [links.txt](datasets/links.txt)
contain the links to all datasets which were found. 

Additionally, the [download_datasets.py](datasets/download_datasets.py) script downloads all easily downloadable datasets. 
Note that some datasets still need to be downloaded manually. Those will be mentioned on execution.
Run the script via 
```
python -m download_datasets
```
inside the datasets folder. 

### Running ES tests

#### Setting up ES
To repeat the ES-Index EL tests, one has to set up an ES instance. 
To populate the ES index, one can execute the methods found in [populate_ES.py](utilities/populate_ES.py). 
Most important is `populate_entities_elasticsearch` which is run on default if 
```
python -m utilities.populate_ES
``` 
is executed. 
One has to provide a n-triples files containing only label statements about entities 
(Default filename `labels.nt` but can be specified by `--filename`). \
To get such pre-filtered n-triple files, the methods in the [filer_labels.py](utilities/filter_labels.py) file can be used. 
The methods expect an existing Wikidata n-triples file and if run via 
```
python -m utilities.filter_labels {filename}
```
creates two files, `labels.nt` and `description.nt`.

#### Running the tests
After setting up the ES instance, the tests can be run by executing [es_el_tests.py](dataset_evaluation/scripts/es_el_tests.py)
via 
```
python -m dataset_evaluation.scripts.es_el_tests
```
.
The datasets have to be placed in the right folder as defined in [paths.py](dataset_evaluation/scripts/paths.py) via the 
`datasets_path` variable.

All generated results can be reprocessed via 
```
python -m dataset_evaluation.scripts.reprocess_es__results {folder with results}
```
to compute additional statistics.

#### Existing results

The existing results can be found in the [es](dataset_evaluation/results/es) folder for each dataset.

### Gathering datasets statistics 

After downloading all datasets and placing them in the folder specified by the 
`datasets_path` variable in [paths](dataset_evaluation/scripts/paths.py), one can execute the analysis script via
[analyse_datasets.py](dataset_evaluation/scripts/analyse_datasets.py) with 
```
python -m dataset_evaluation.scripts.analyse_datasets
```
.

#### Existing results

The existing results can be found in the [analysis](dataset_evaluation/results/analysis) folder for each dataset.


## KG evaluation

### Preparation

One has to download n-triples dumps of the different KGs (DBpedia and Wikidata) and the json dump of Wikidata.

### Generating the results

There exist multiple scripts to generate statistics. To calculate the mention overlap, [calculate_mention_overlap.py](kg_evaluation/scripts/calculate_mention_overlap.py) 
and [calculate_mention_overlap_wikidata.py](kg_evaluation/scripts/calculate_mention_overlap_wikidata.py) can be used. 
Both expect a n-triples file. The main difference is that `calculate_mention_overlap_wikidata.py` can only use a pre-filtered Wikidata dump containing labels. 
The first is executed via 
```
python -m kg_evaluation.scripts.calculate_mention_overlap {filename} {mention_dictionary_filename} {results_filename}
```
and the second via 
```
python -m kg_evaluation.scripts.calculate_mention_overlap_wikidata {filename}
```
Both generate also a mention dictionary file which is in the case of Wikidata further used via the 
[wikidata_label_length.py](kg_evaluation/scripts/wikidata_label_length.py) script with 
```
python -m kg_evaluation.scripts.wikidata_label_length {mention_dict_file}
```
. It calculates statistics like the average 
mean, median or p-percentile mention length.

Language-wise statistics of Wikidata are generated by [extract_language_statistics.py](kg_evaluation/scripts/extract_language_statistics.py). 
It expects a Wikidata dump in the json format.
The generated files can be reprocessed by executing  [reprocess_language_statistics.py](kg_evaluation/scripts/reprocess_language_statistics.py)
 to generate additional statistics.

### Existing results

The existing results can be found in [results](kg_evaluation/results).

## Other scripts

All scripts and additional data used for plotting can be found in [plotting](plotting). 
Also, all plots are stored there as well.