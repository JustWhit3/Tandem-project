## Reference guides folder
### General informations
This folder contains all the informations related to the classes and libraries used for the program.

### List of the documents in the folder
#### AMS_functions.py ([Link Here](https://github.com/JustWhit3/Software_and_Computing_program/blob/master/Reference%20guides/AMS_functions.py))

This document contains the definition and explanation of the AMS functions.
AMS metric is used for the evaluation of my model. To see its detailed definition see the [PDF_dataset.pdf](https://github.com/JustWhit3/Software_and_Computing_program/blob/master/Explanations/PDF_dataset.pdf) document into the [Explanations](https://github.com/JustWhit3/Software_and_Computing_program/tree/master/Explanations) folder. The complete formula is:

<img src="https://render.githubusercontent.com/render/math?math=AMS = \sqrt{2 \left( (s %2B b %2B b_{reg}) ln\left(1 %2B \dfrac{s}{b %2B b_{reg}} \right) -s \right)}">

where:
+ <img src="https://render.githubusercontent.com/render/math?math=s = \sum_{i} w^{s}_{i}">  is the unormalized sum of signal weights.
+ <img src="https://render.githubusercontent.com/render/math?math=b = \sum_{i} w^{b}_{i}">  is the unormalized sum of background weights.
+ <img src="https://render.githubusercontent.com/render/math?math=b_{reg} = 10">  is a regularization therm that reduces the variance of the AMS.

In this document have been defined two functions:

1) `NN_output_to_AMS`: 
this function takes 4 arguments:
+ "x_cut" is the cut parameter of the AMS. It ranges from 0.5 to 1 in steps of 0.1.
+ "predictions" is a binary array, defined from the set of data that we're considering (for ex: validation set).
+ "label_vectors" is a binary array constructed from the dataset, used for each model, that distinguishes an event between signal and background.
+ "weights" this takes the weights associated to each data of my dataset (in my case the "KaggleWeight").

2) `plot_AMS`: this function takes similar arguments of the previous one. It uses the previous one to plot the final result of the AMS.

#### Plot_distributions.py ([Link Here](https://github.com/JustWhit3/Software_and_Computing_program/blob/master/Reference%20guides/Plot_distributions.py))

This document contains the definition and explanation of the "plot_distribution" functions.
There are two functions defined in my program: `plot_distributions` and `plot_distributions_final`.This functions are useful for the plotting of the distributions of each model. I'll explain only the second one, because it's more complete and extended in respect to the first one. So, this one takes 7 arguments:

+ "prediction_val" that are prediction data for the validation set. It's a 2-dim array.
+ "prediction_test" that are prediction data for the test set. It's a 2-dim array.
+ "true_val" that are the output data of the model (for the validation set). It's a 2-dim array.
+ "n_bins" that are the number of bins (usually set to 50). It's an integer.
+ "weighted" is a boolean variable set to be True if the histogram is weighted, otherwise if it's unweighted.
+ "weights_val" in case in which my histogram is weighted this are the weights of the validation data.
+ "weights_test" and this are the weights of the test data.

#### Make_model.py ([Link Here](https://github.com/JustWhit3/Software_and_Computing_program/blob/master/Reference%20guides/Make_model.py))

This document contains the definition and explanation of the "make_model" functions. It has been defined this function, in a way to avoid to repeat, everytime you define a new DNN model, the same code. This function takes 7 arguments:

+ "layer_sizes" is related to the size of the layers of the network. It takes a list of integers.
+ "activation" is related to the activation function that you use. It takes a string with the name of the activation function.
+ "dropout rate" this is the rate of the dropout, if 0, there will be no dropout.
+ "optimizer" this takes a string with the name of the optimizer you want to use.
+ "regularization" this takes a string with the name of the regularizer you want to use.
+ "input_dimension" this takes the shape of the input data.

#### Splitting_function.py ([Link Here](https://github.com/JustWhit3/Software_and_Computing_program/blob/master/Reference%20guides/Splitting_function.py))

This document contains the definition and explanation of the "splitting" function. This function is useful for the splitting of the dataset into a subset (train, test or validation set). In particular, given a certain dataset, this gives:
+ The selected subset that you want.
+ The binary vectors both for the networks and the BDT.
+ The weights associated to the validation and test set.

In this function it has been applyied also an operation on the feature enginnering:
The problem is that the "phi" variables, in the Kaggle dataset, have a signal distribution that is very similar to the background one. So it's better to consider their linear combination (difference in this case) to make them useful in my classification:
+ `Delta_phi_tau_lep` = `PRI_tau_phi` - `PRI_lep_phi` (not helpful for 2 jets category)
+ `Delta_phi_met_lep` = `PRI_met_phi` - `PRI_lep_phi`
+ `Delta_phi_jet_jet` = `PRI_jet_leading_phi` - `PRI_jet_subleading_phi`
    + Drop `PRI_tau_phi`,  `PRI_lep_phi`, `PRI_met_phi`, `PRI_jet_leading_phi` and `PRI_jet_subleading_phi`

It depends on 2 variables:
+ "dataset": it's the name of the dataset. It receives a DataFrame from Pandas.
+ "string": it's related to the the kind of subset you want. It's a string, you've to put the letter of the test, training or validation set in my case..

#### Splitting_jets_function.py ([Link Here](https://github.com/JustWhit3/Software_and_Computing_program/blob/master/Reference%20guides/Splitting_jets_function.py))

This document contains the definition of two functions:
1) `preprocessing_function`: it's used for the preprocessing of the dataset, used before applying all the more significant cuts on its features (like the dropping of some of them). This function sets the -999 value of `MMC` variable to the mean improved classification and drops the unuseful features from the subset. It takes only one argument, that is the previous mentioned subset (pandas DataFrame).

2) The function for the splitting into jets (for the DNNs only): `splitting_jets`. This function is useful to split my dataset, keeping into account the number of jets, in the final state of the Higgs boson decay. The dataset is divided into three sets of data: one for the 0 jets case, one for the 1 jet case and one for the 2 or 3 jets case. This function takes 4 arguments:
+ "subset": is one of the set obtained from the previous splitting of the dataset with the `splitting` function. It's a pandas DataFrame.
+ "y_subset": is the binary array for the subset.
+ "weights": is the weight of the validation or test set.
+ "jets_number": indicates the number of jets. It's an integer.

### Function testing
For the function testing has been used the tool `pytest`.
If you want to test the functionality of the functions you've to do this passages:
1) Download the pytest tool from your shell, typing: `pip install pytest`.
2) Write on the shell: `pytest <function>.py` where <function<function>> represents the name of the function you want to test and ".py" is the file extension.
