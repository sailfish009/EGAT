# EGAT
This repository contains the official implementation of our paper *"EGAT: **E**dge Aggregated **G**raph **At**tention Networks and Transfer Learning Improve Protein-Protein Interaction Site Prediction"*.

If you use any part of this repository, we shall be obliged if you site our paper.

# Usage
## Pytorch and DGL installation
We implemented our method using PyTorch and Deep Graph Library (DGL). Please install these two for successfully running our code. Necessary installation instructions are available at the following links-
1. [Python 3.6 or later version](https://www.python.org/downloads/)
2. [PyTorch](https://pytorch.org/get-started/locally/#start-locally)
3. [Deep Graph Library](https://www.dgl.ai/pages/start.html)

## Download pretrained-model weights:
### ProtBERT model weight
1. Please download the pretrained model weight-file "pytorch_model.bin" from [here](https://drive.google.com/file/d/10MLado6OTLtQ_RWbBEyZNaPCVXaCf73z/view?usp=sharing).
2. Place this weight-file in the folder "EGAT/inputs/ProtBert_model".
If you use this pretrained model for your paper, please cite the paper [ProtTrans: Towards Cracking the Language of Life’s Code Through Self-Supervised Deep Learning and High Performance Computing](https://www.biorxiv.org/content/10.1101/2020.07.12.199554v2)
### EGAT model weight
1. Please download the pretrained model weight-file "egat_model_weight.dat" from [here](https://drive.google.com/file/d/1KsbJ6x8y_8YneO29d81khIhXwt57SWwz/view?usp=sharing).
2. Place this weight-file in the folder "EGAT/models".

## Input Data
To store input-features, navigate to the folder "EGAT/inputs". In this folder, follow any of the following steps:
1. Store the PDB files of the isolated proteins that shall be used for prediction in the folder "pdb_files". Rename the PDB files in the format: "\<an arbritary name\>\_\<chain IDs\>". Please see the example PDB files provided in this folder. Please provide the real chain IDs (as available in the PDB file) after the underscore ("\_") correctly. (In the provided examples \<an arbritary name\> is the PDB ID of a complex in which this input protein is one of the subunits. It is not mendatory.)
2. List all the protein-names in the file "protein_list.txt"

## Run inference to predict numeric propensity (of each of the residues) for interaction
1. From command line cd to "EGAT" folder (where the "run_egat.py" file is situated).
2. Please run the following command:
```python
python run_egat.py
```
3. The command above will generate the results in the "EGAT/outputs" folder.

# Output format
1. The output generated by running EGAT will be stored as a [*pickle file*](https://docs.python.org/3/library/pickle.html) in the "EGAT/outputs" folder. To open the pickle file please run the following commands in the python interpreter:
  ```python
  import pickle_
  output = pickle.load(open('EGAT/outputs/prediction_and_attention_scores.pkl', 'rb'))
  ```
2. In the above commands the *"output"* variable is a [*python dictionary*](https://docs.python.org/3/tutorial/datastructures.html#dictionaries) (with the four keys: 'pred', 'protein_info', 'edges', 'attention_scores').  
  - To access the predicted numeric propensity, please run the following commands:  
    ```python
    prediction = output['pred']  
    protein_index = 0  
    print(prediction[protein_index])  
    ```
    In the above commands, "protein_index" represents the index of the protein-name in the "protein_list.txt" file. (You can set it to any number, e.g: for the protein-name at index 2 (third row of the "protein_list.txt" file), set protein_index=2).  
    These commands will print the predicted numeric propensities of all the residues in the protein at index "0" of "protein_list.txt" file. The propensities will be printed sequentially following the order of the residues in the input PDB file of this protein.  

  - To access general information about the input proteins, please run the following commands:  
    ```python
    protein_information = output['protein_info']  
    protein_index = 0  
    print(protein_information[protein_index])    
    ```
    These commands will print a [*python dictionary*](https://docs.python.org/3/tutorial/datastructures.html#dictionaries) corresponding to the protein at index "0" of "protein_list.txt" file. This python dictionary contains the number of residues in the protein (represented with the key 'seq_length' in this dictionary).  

  - To access the edges of the graphs representions of the input proteins, please run the following commands:  
    ```python
    graph_edges = output['edges']  
    protein_index = 0  
    print(graph_edges[protein_index])  
    ```
    These commands will print a [*numpy array*](https://numpy.org/doc/stable/reference/generated/numpy.array.html) corresponding to the protein at index "0" of "protein_list.txt" file. Each row of this *numpy array* corresponds to a *neighborhood*, that contains the indices of the neighboring nodes (residues) of one residue (i.e. *the center of the neighborhood*). (please see [our paper]() for more details). This *center of the neighborhood* is the row count of the matrix. The following example command will print the *neighborhood* (neighboring residue indices) of the residue with index *2* -  
    ```python
    center_node = 2  
    print(graph_edges[protein_index][center_node])  
    ```
  
  - To access the attention scores associated with the edges, please run the following commands:  
    ```python
    attention_scores = output['attention_scores']  
    protein_index = 0  
    print(attention_scores[protein_index])  
    ```
    These commands will print a [*numpy array*](https://numpy.org/doc/stable/reference/generated/numpy.array.html) corresponding to the protein at index "0" of "protein_list.txt" file. Each row of this *numpy array* contains the attention scores associated with the corresponding edge. In the following example command, *center of the neighborhood* is the residue at position *2*. This command will print the attention scores associated with the edges from its neighboring residues (nodes) to this residue-  
    ```python
    center_node = 2  
    print(attention_scores[protein_index][center_node])  
  ```
  
  
  
