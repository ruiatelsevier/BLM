# BLM

<b>Bilinear Link Model (BLM)</b> is a probabilistic model for <a href="https://en.wikipedia.org/wiki/Feature_learning">learning representations</a> of nodes in directed networks.
BLM is described in <a href="http://sites.skoltech.ru/app/data/uploads/sites/34/2015/07/ivanov_bartunov_aist2015.pdf">Learning Representations in Directed Networks</a> paper which was presented on <a href="http://aistconf.org/2015">AIST'2015 conference</a>.
This is a scalable parallel implementation of the proposed in the paper learning representation algorithm.

### Installing

For building the binaries you need C++ compiler (g++) and POSIX threads on your system.

Run ```g++ -O2 -pthread BLM.cpp -o BLM``` to compile code.

### Usage

There is only one way to run the algorithm: ```./BLM config.cfg```. All parameters are contained in config.cfg.

 - ```network_filename``` is required parameter contains the name of file with network (see network file format description below).
 - ```vector_size``` is required integer parameter corresponds to learned representations dimentionality.
 - ```nu``` is required positive integer parameter responsible for number of NCE objects generated on each SGA iteration. Values from 5 to 30 are recommended.
 - ```number_of_epoch``` is required positive integer parameter which determinates the number of SGA optimization epochs.
 - ```number_of_threads``` is required positive integer parameter which determinates the number of threads for parallel SGA optimization. For best performance use the number of cores of your computer for this setting.
 - ```eta``` is requeried positive float parameter corresponds to learning rate in SGA optimization method. Do not tune it without adequate understanding of its meaning, because it may break down the convergence of method. Value ```0.01``` worked fine on test networks.
 - ```gamma``` is required positive float parameter responsible for regularization. Increase of this parameter causes learned representations to be closer to zero. Decrease of this parameter causes representations to be far away from zero. Value ```0.003``` worked fine on test networks.
 - ```output_textfile_model_filename``` is optional parameter contains the name of file with results of learning (see result file format description below).
 - ```output_textfile_model_format_string``` is optional parameter (but required with ```output_textfile_model_filename```), which contains C-printf-style format string for writting float is results file.
 
#### Network file format
Each line of this file contains one edge in the following format: source node label, destination node label, and optionally edge weight (by default weight is 1.0).
 
Node label is a string without any whitespace characters. All node labels must be unique.
 
#### Output file format
 
First line contains two integers: number of nodes and length of their learned vector representations.

Following lines contains node respresentations.

Node representation takes four lines:
- first line contains the node label
- second line contains ```In``` representation of the node
- third line contains ```Out``` representation of the node
- fourth line contains normalization constant ```Z``` of the node

One can use both ```In``` abd ```Out``` representations of the node, but in some cases ```Out``` representation is more informative, so it is better to use it. For more details see original BLM paper.
