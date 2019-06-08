# Depression
Model for Bayesian approach to estimate saFC

This document explains how to use the python code to estimate saFC (spatailly-adjusted functional connectivity) in MDD research. 
Our manuscript entitled "A Bayesian Approach to Examining Default Mode Network Functional Connectivity and Cognitive Performance 
in Major Depressive Disorder" under review used the code to estimate FC by integrating naive FC and structural connectivity (SC). 
More details about the model and code can be found at http://hakmook.com/2018/07/02/double-fusion-model-with-pymc3.html 
written by my former student Dr. Rui Wang. The Bayesian double fusion model was proposed by Kang et al., Brain Connectivity (2017): 219-227. 

To run the code, you have to install python, pymc3, and theano as described in http://hakmook.com/2018/07/02/double-fusion-model-with-pymc3.html.
Then you need to convert your reesting state fMRI into [(number of voxels x number of ROIs)] x T where T = length of time series. 
In our case, we have 300 voxels (randomly chosen) per ROI and T = 150. Save this data as "ROI_timeseres_data.csv". 
For naive FC, you simply compute the correlation matrix with the averaged time series in each ROI. In our study, we have 14 ROIs, 
meaning that we have 14 by 14 correlation matrix. Then extract the upper diagonal matrix and convert it to a row vector (1 x 91) 
in the order of FC(1,2), FC(1,3), FC(2,3),..., FC(12,14), and FC(13,14). Save this vector as "DMN_MeanFunctional_Connectivity.csv". 
Please do the same for structural connectivity vector and save it as "DMN_StructuralConnectivity.csv", Please note that the structural 
connectivity vector is a column vector (91 x 1).
In each ROI, you need to compute the euclidean distance matrix for all the voxels in the ROI. In our case, we have 300 by 300 
distance matrix. Then save this matrix as "distMatrix_ROI_x.csv", where x = 1,...,14. This matrix is used to estimate the parameters 
associated with spatial covariance function. For simplicity, we used the exponential covaraince function 
(see line 163, kernel="exponential"). You may want to try other covariance functions, e.g., "gaussian", "matern52", and "matern32", 
by simply saying kernel= one of those three.
In line 164, you provide the number of ROIs. In our case, we have n = 14.
In lines 165 and 166, you simply provide the number of posterior samples and tuning samples. With sample_size = 1000 and tune_size = 1000, 
we collect 1000 posterior samples preceded by 1000 tuning samples. Please find more details at https://pymc3.readthedocs.io/en/latest/.

Before you run, please provide your input and output directories in lines 157 & 158. Index_list = [8003] is not necessary 
but is useful to process multiple subjects' data (please see line 54).

If you are a Mac user, then run the code in terminal,
python subject_8003_model.py
