# Surging potential and controlling parameters for glaciers in Svalbard
This README contains where to find the information to reproduce the figures of the paper Bouchayer et al., (in prep).
First, the code contains how to handle the data

## Data used
-Ice free topography: Fürst et al., 2018 -  https://data.npolar.no/dataset/57fd0db4-afbf-4c94-ac1c-191c714f1224

-SVIFT database: van Pelt etla., 2019 - https://figshare.com/articles/dataset/A_long-term_dataset_of_climatic_mass_balance_snow_conditions_and_runoff_in_Svalbard_1957-2018_/7836530/1

-Geometrical width computed with OGGM - https://cluster.klima.uni-bremen.de/~oggm/assets/oggm_v1.4/centerlines_shapes

-Python package for OGGM: https://github.com/OGGM/oggm

## Data Manipulation - /data_set
### coordinates_centerlines.ipynb
The Jupyter notebook use OGGM model (Maussion et al., 2019) to compute the centerlines coordinates for each glaciers in Svalbard

### data_interp_centerlines.ipynb
We import the data of the bed elevation from the SVIFT database (Fürst et al., 2018).
We import the geometrical widths calculated with OGGM.
We import climatic data (runoff, ELA, beta and CMB) from van Pelt et al., 2019. 
We interpolate these data on the centerline points.

### data_set_slope_point.ipynb
From the dataframe computed before, we calculate the surface and bed slope in between each point

### data_set_for_modelling.ipynb
For machine modelling, we add some varibales: dummy varibales (no physical meaning, value between 0 and 1 allocated randomly), the thickness / width  and the width x thickness x bed slope. We recalculate the SMB using the ELA and the SMB gradient (gradient x (surface elevation - ELA)). We drop the glaciers that are shorter than 1km and the ones that have a area less than 1km2. We drop the glaciers that have a surge index equals to 9 in the RGI classification and we convert surge classes (3 - observed surge becomes 1).

## Machine learning - /machine_learning
### /machine_learning/model_evaluation
#### RF_xgb_LR_surf_slope_trainGlaciers.ipynb
Comparison between random forest, logistic regression and XGBoost. The AUCs, precision-recall, confusion matrix are calculated after having performed a resampling of the data. After the resampling, we select 70% of the entire centerlines for training the models and 30% for testing the models. 
#### combine_roc_plots.ipynb
The Jupyter notebook is assembling all the roc plot for the 3 different models. 

### /machine_learning/probability_map/
#### probability_map.ipynb
The code takes the results of XGBoost to produce probability maps of the surging potential of glaciers in Svalbard. The code produces first, the probability of each points to be surge-type, then the same map but only for Austfonna region, then the probability map average for each glaciers. The cumulative probability curve is calculated as well as the disctribution of the probability. At the end of the jupyter notebook, a comparison between RGI classes and our probability is performed. 

### /machine_learning/feature_importances/
#### feature_importances.ipynb
This Jupyter Notebook is computing 50 bootstrap simulations for the three models (logistic regression, random forest, XGBoost). For the 50 simulations, the feature importances scores are calculated. The scores are then normalised and multiplied by the AUC. For XGBoost, the gain and the weight feature importances are computed. The last part of the code performs a recursive feature addition. Taking the order of the feature importances scores, each features are added recursively to the model and the AUC is caluclated. 

### /machine_learning/Grid_search_parameters/
#### exhaustive_grid_search.ipynb
The Jupyter notebook is performing an ehaustive grid search for the mthree models we are using. It search across a range of hyperparameters the ones that are producing the best performance of our models. 

### /machine_learning/SHAP_values/
#### shap_values.ipynb
This notebook compute the shap values (summary plot) for XGBoost model.

# Cite this work
@TODO once published


