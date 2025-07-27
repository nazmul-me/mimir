## Instructions

First install the python dependencies

```
pip install -r requirements.txt
```

Then, install our package

```
pip install -e .
```

To use, run the scripts in `scripts/bash`

**Note: Intermediate results are saved in `tmp_results/` and `tmp_results_cross/` for bash scripts. If your experiment completes successfully, the results will be moved into the `results/` and `results_cross/` directory.**

## Setting environment variables

You can either provide the following environment variables, or pass them via your config/CLI:

```
MIMIR_CACHE_PATH: Path to cache directory
MIMIR_DATA_SOURCE: Path to data directory
```

## Using cached data

The data we used for our experiments is available on [Hugging Face Datasets](https://huggingface.co/datasets/iamgroot42/mimir). You can either choose to either load the data directly from Hugging Face with the `load_from_hf` flag in the config (preferred), or download the `cache_100_200_....` folders into your `MIMIR_CACHE_PATH` directory.

## MI methods (LOSS, MIN_K, ZLIB, REF.) experiments how to run

```
python run.py --config configs/mi.json
```

We collected all the results in the `results_new` folder. In addition, we created another `ModelsResults.xlsx` file from all CodeLLMs' results in the `RQsResult/data` folder to answer RQs and produce figures.

# MI methods

We include and implement the following MI methods, as described in our paper.

- [Likelihood](https://ieeexplore.ieee.org/stamp/stamp.jsp?arnumber=8429311) (`loss`). Works by simply using the likelihood of the target datapoint as score.
- [Reference-based](https://arxiv.org/abs/2004.15011) (`ref`). Normalizes likelihood score with score obtained from a reference model.
- [Zlib Entropy](https://www.usenix.org/system/files/sec21-carlini-extracting.pdf) (`zlib`). Uses the zlib compression size of a sample to approximate local difficulty of sample.
- [Min-K% Prob](https://swj0419.github.io/detect-pretrain.github.io/) (`min_k`). Uses k% of tokens with minimum likelihood for score computation.

## Adding your own dataset

To extend the package for your own dataset, you can directly load your data inside `load_cached()` in `data_utils.py`, or add an additional if-else within `load()` in `data_utils.py` if it cannot be loaded from memory (or some source) easily. We will probably add a more general way to do this in the future.

## Adding your own MI method

To add an MI method, create a file for your MI method (e.g. `attacks/my_attack.py`) and implement the interface described in `attacks/all_attacks.py`.
Then, add a name for your MI method to the dictionary in `attacks/utils.py`.

If you would like to submit your MI method to the repository, please open a pull request describing your MI method and the paper it is based on.

# Task Performance

## Prerequisite: installation

```
pip install codebleu
pip install tree-sitter-python
```

For more information, follow https://pypi.org/project/codebleu/

## Evaluation

Go to `taskEvaluation` folder and run the following-

```
python3 codeBLEU_pythia.py
```

Here, we have to provide two values. One of them is the name of the CodeLLM (e.g., `EleutherAI/pythia-70M`) in `Line no 25` and second one is the precision of the CodeLLM (e.g., 8-bit, 4-bit) in `Line no 10`.

This results are also saved `ModelsResults.xlsx` file along with all CodeLLMs' MI results in the `RQsResult/data` folder to answer RQs and produce figures.

# Results of the preliniary, RQs and findings

Go to `RQsResult` folder and run the following ipynb file.

Preliminary Results (Section 2.3):

```
PreliResult.ipynb
```

RQ-1:

```
RQ1-precisionVSmia.ipynb
```

RQ-2:

```
RQ2-taskVSmia.ipynb
```

RQ-3:

```
RQ3-generalizibilty.ipynb
```

Findings part results:

```
Findings.ipynb
```

Significance Test results:

```
SignificanceTest.ipynb
```
