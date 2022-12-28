**OVERVIEW**

Three main experiments were proposed, each comprising the same designed model but using a variety of 
genetic and non-genetic features to generate the Genetic Risk Scores. Each experiment was divided into 
two, catering for further combinations of features used in each. The following list shows the specific 
variables included within each experiment section (each section was again separated to cater for 
differences in males and females):

Experiment 1 Section A - 7 SNPs/polymorphisms
Experiment 1 Section B - 7 SNPs from 1A + 11 added SNPs
Experiment 2 Section A - 5 main environmental risk factors for MI
Experiment 2 Section B - 5 main environmental risk factors from 2A + 4 added environmental risk factors
Experiment 3 Section A - 5 main environmental risk factors from 2A + all SNPs
Experiment 3 Section B - All environmental risk factors from 2B + all SNPs

For each experiment section, three main tasks have been conducted (the file suffixes are included in 
brackets which correspond to each task):

Task 1 - Generating GRSs using a linear regression model (_linear.ipynb)
Task 2 - Running Task 1 100 times to generate a distribution of results (_distribution.ipynb)
Task 3 - Generating GRSs using 3 non-linear regression models (_non_linear.ipynb)

File names have the following structure:
Experiment number_Experiment section_Sex_Task.ipynb

==TASK 1==

*PREREQUISITES:

The file required for Task 1 in all of the experiments is "mami_data_file.csv". This contains the raw MAMI 
study data which include biochemical tests and genotype data, together with environmental and lifestyle 
factors. For sections 1B, 3A and 3B an additional two files are needed depending on the sex listed on the 
file; "Female_genos.csv" and "Male_genos.csv". These two files contain different genotype data for 
men and women which have been selected through the second selection approach described in the 
methodology.

*DESCRIPTION:

After a set of preprocessing steps, a multiple linear regression model is fitted with Triglycerides as the 
dependent variable (target) and the respective features for each experiment as the independent variables. 
The model was trained on 80% of the controls and tested for performance on the remaining 20%. A 
prediction was then obtained on all of the data (both cases and controls). This generated a list of predicted 
triglycerides, a new variable, which is weighted by the respective independent variables. The predicted 
triglyceride variable was grouped into quartiles according to the distribution of the controls. This 
generated a variable with 4 groups, each representing a GRS. To calculate the odds ratio of each GRS 
group with MI, a logistic regression model was fit. MI was set as the target and the GRS variable (i.e. 
categorised predicted triglycerides) was set as the independent feature.

==TASK 2==

*PREREQUISITES:

In Task 2, the same exact file requirements of Task 1 are needed.

*DESCRIPTION:

The same code as that of Task 1 was run with a single difference. To test whether the resulting odds ratios 
were reproducible, the random_state parameter within the train_test_split function was set as a random 
integer, and the whole code was ran into a for loop iterating 100 times, each time changing the 
random_state value and recording a dataframe of OR values. The OR values which correspond to each 
GRS group were plotted in a box plot to visualise the distributions. This showed whether having different 
random data splits varies the results to a great extent. 

==TASK 3==

*PREREQUISITES:

In Task 3, the same exact file requirements of Task 1 and 2 are needed.

*DESCRIPTION:

Three non-linear supervised regression models namely, Random Forest (RF), Support vector machine 
(SVM) and multi-layer perceptron (MLP) regression models were used in this task, replacing the multiple 
linear regression in the pipeline to compare their performances. The grid search approach was 
implemented in GridSearchCV class of the sklearn.model_selection package to find optimal 
hyperparameters and to improve performance in the three regression models. Firstly, the control dataset 
used for training was divided into 2 using 2-fold cross-validation since the sample size was already very 
small. Secondly, the hyperparameter search space was defined by defining a dictionary with important 
parameters for each of the three regression models. The best hyperparameter combination that had the 
lowest MSE and highest R2 was then chosen to train the models. This was done to see if the performance 
of the model improves compared to the linear model. The rest of the steps were kept the same to create a 
comparable output.  

