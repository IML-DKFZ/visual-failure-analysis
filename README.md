# Purpose
This toolkit is designed to analyse the underlying representation of a image classification network.  
It is designed to be compatable with the outputs of the https://arxiv.org/abs/2211.15259 Failure Detection Benchmark described in this paper (https://github.com/IML-DKFZ/fd-shifts).

# Instructions
1) Create a new fodler  
2) Create a new virtual environment  
3) Update your pip  
4) Install the package (pip install  pip install visualfailureanalysis)  
5) Setup your folder in the form "../experiemnt_group_name/experiment_name/test_results"  
6) In this folder place your data as raw_outputs.npz  
7)  Format: Outputs of the softmax in the first columns followed by the label as an integer and dataset integer index in the last column. Each row is one data point. If you only have one data set the index is always 0.  
8) In this folder place your latent space as encoded_output.npz  
9) Format: Outputs of the penultimate layer (e.g. inputs to the final layer) in the first columns and dataset index in the last. Each row is one data point.   
10) In this folder place your data as attribution.csv
11) attributions.csv needs to at least contain a column called "filepath" containing the absolute filepaths of your images. If you have multiple data sets attributions are renamed to "attributions0.csv","attributions1.csv",..  
12) All three data files need the rows to be in the same order!  
13) In python: from visualfailureanalysis import analyser  
14) Initalize the main class: my_data_visulizer = analyser.Analyser(path=path,class2name=class2name,class2plot=class2plot,ls_testsets=ls_testsets,test_datasets=test_datasets)  
15) path ="../experiemnt_group_name/experiment_name"  
16) class2plot = dict({0:"myclassname0",...}) conatianing a mapping of integer classes to real names   
17) ls_testsets = ["nameoftestset",...] a list with names of all testsets  
18) class2plot and test_datasets are two lists with a subset of classes/ testset names for which to generate outputs. (output can be quite large)  
19) run my_data_visulizer.setup() to link the create the lower dimensional representation needed  
20) generate outputs and statistics with the respective class methods.  
Example Tree:
|--|--project  
      |--experiment_group_name  
         |--experiment_name  
            |--test_results  
               |--raw_output.npz  
               |--encoded_output.npz  
               |--attribution.csv  

# Outputs
Generates outputs for representative images based on a k-means clustering of the latent space. Latent space dimenstions are reduced to 50 by pca and further condensed to 3 by T-SNE.   
Generates the most overconfident as well as underconfident images based on the softmax response (could be adjusted to other confidence score).
The app.py file can be run by "python3 app.py" and starts an interactive dash app that allows the user to explore the latent space of his neural network and the images linked to each data point. The data necessary to run the app can be generated by the package. After initalizing the main analyser simply run analyzer.prepaire_dash() and the correct data frame is written.
