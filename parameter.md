## PDPbox functions and parameters

#### pdpbox.pdp.pdp_isolate(model, train_X, feature, num_grid_points=10, percentile_range=None)
function to calculate PDP plot for a single variable 
  
**Parameters:** 
  
* **model**: scikit-learn model    
  - a fitted scikit-learn model  

* **train_X**: pandas DataFrame  
  - dataset on which the model is trained  

* **feature**: string or list  
  - column to investigate (for one-hot encoding features, a list of columns should be provided)  

* **num_grid_points**: integer, default=10  
  - number of grid points for numeric features  

* **percentile_range**: (low, high), default=None  
  - percentile range to consider for numeric features  

**Example:**  
```python
from pdpbox import pdp
# for binary feature
pdp_sex = pdp.pdp_isolate(clf, titanic[features], 'Sex')

# for onehot encoding feature
pdp_embark = pdp.pdp_isolate(clf, titanic[features], ['Embarked_C', 'Embarked_S', 'Embarked_Q'])

# for numeric feature
pdp_fare = pdp.pdp_isolate(clf, titanic[features], 'Fare')
# more options
pdp_fare = pdp.pdp_isolate(clf, titanic[features], 'Fare', num_grid_points=20)
pdp_fare = pdp.pdp_isolate(clf, titanic[features], 'Fare', num_grid_points=20, percentile_range=(5, 95))
```
-------------------------------------------------------------------------------------------------------
    
#### pdpbox.pdp.pdp_interact(model, train_X, features, num_grid_points=[10, 10], percentile_ranges=[None, None])
function to calculate interaction plot for two variables 
  
**Parameters:**
  
* **model**: scikit-learn model    
  - a fitted scikit-learn model  

* **train_X**: pandas DataFrame  
  - dataset on which the model is trained  

* **feature**: list  
  - a list containing two features to investigate  

* **num_grid_points**: list, default=[10, 10]  
  - a list of number of grid points for each feature  

* **percentile_ranges**: list, default=[None, None]  
  - a list of percentile range to consider for each feature  

**Examples:**  
```python
from pdpbox import pdp
# simple interaction between two numeric features
inter1 = pdp.pdp_interact(clf, titanic[features], ['Age', 'Fare'])

# interaction between binary feature and numeric feature
inter2 = pdp.pdp_interact(clf, titanic[features], ['Sex', 'Age'], num_grid_points=[None, 10])

# interaction between onehot encoding feature and numeric feature
inter3 = pdp.pdp_interact(clf, titanic[features], [['Embarked_C', 'Embarked_S', 'Embarked_Q'], 'Age'], num_grid_points=[None, 10])
```
----------------------------------------------------------------------------------------

#### pdpbox.pdp.pdp_plot(pdp_isolate_out, feature_name, center=True, plot_org_pts=False, plot_lines=False, frac_to_plot=1, cluster=False, n_cluster_centers=None, cluster_method=None, x_quantile=False, figsize=None, ncols=None, plot_params=None, multi_flag=False, which_class=None)
  
**Parameters:**
  
* **pdp_isolate_out**: instance of pdp_isolate_obj  
	- a calculated pdp_isolate_obj instance  
  
* **feature_name**: string  
	- name of the feature, not necessary the same as the column name  

* **center**: boolean, default=True  
	- whether to center the plot

* **plot_org_pts**: boolean, default=False
	- whether to plot out the original points

* **plot_lines**: boolean, default=False
	- whether to plot out the individual lines

* **frac_to_plot**: float or integer, default=1
	- how many points or lines to plot, can be a integer or a float

* **cluster**: boolean, default=False
	- whether to cluster the individual lines and only plot out the cluster centers

* **n_cluster_centers**: integer, default=None
	- number of cluster centers

* **cluster_method**: string, default=None
	- cluster method to use, default is KMeans, if 'approx' is passed, MiniBatchKMeans is used

* **x_quantile**: boolean, default=False
	- whether to construct x axis ticks using quantiles

* **figsize**: (width, height), default=None
	- figure size

* **ncols**: integer, default=None
	- used under multiclass mode

* **plot_params**: dict, default=None
	- values of plot parameters 
    
	**plot_params possible values and description:**
	- **'font_family'**: font_family for the plot, default='Arial
	- **'title'**: title for the plot, default=ICEplot for (feature_name)
	- **'title_fontsize'**: title font size, default=15
	- **'subtitle_fontsize'**: subtitle font size, default=12
	- **'pdp_color'**: color for PDP line, default='#1A4E5D'
	- **'pdp_hl_color'**: color for PDP line highlight, default='#FEDC00'
	- **'pdp_linewidth'**: width of PDP line, default=2
	- **'zero_color'**: color of the zero line, default='#E75438'
	- **'zero_linewidth'**: width of the zero line, default=1.5
	- **'fill_color'**: color of the standard deviation fill, default='#66C2D7'
	- **'fill_alpha'**: alpha of the standard deviation fill, default=0.2
	- **'markersize'**: size of the PDP marker, default=5
	- **'point_size'**: size of the original points, default=50
	- **'point_pos_color'**: color of the positive point, default='#5BB573'
	- **'point_neg_color'**: color of the negative point, default='#E75438'
	- **'line_cmap'**: matplotlib color map for individual lines, default='Blues'
	- **'cluster_cmap'**: matplotlib color map for clustered lines, default='Blues'
	
	- Example:
	```python
	plot_params = {
		'font_family': 'Helvetica',
		'title_fontsize': 16,
		'pdp_color': 'orange',
		...
	}
	```

* **multi_flag**: boolean, default=False
	- whether it is a subplot of a multiclass plot

* **which_class**: integer, default=None
	- which class to plot

**Examples:**
```python
from pdpbox import pdp
pdp_sex = pdp.pdp_isolate(clf, titanic[features], 'Sex')

# basic plot
pdp.pdp_plot(pdp_sex, 'sex')

# more options
pdp.pdp_plot(pdp_sex, 'sex', plot_org_pts=True)
pdp.pdp_plot(pdp_sex, 'sex', plot_org_pts=True, plot_lines=True, frac_to_plot=0.5)
pdp.pdp_plot(pdp_sex, 'sex', center=True, plot_org_pts=True, frac_to_plot=0.5, cluster=True, n_cluster_centers=10)
pdp.pdp_plot(pdp_sex, 'sex', plot_org_pts=True, plot_lines=True, frac_to_plot=0.5, figsize=(6, 6))

# customized plot
plot_params = {
    'pdp_color': 'green',
    'fill_color': 'lightgreen'
}
pdp.pdp_plot(pdp_sex, 'sex', plot_org_pts=True, frac_to_plot=0.5, plot_params=plot_params)

# for multiclass
pdp_feat_67 = pdp.pdp_isolate(clf, otto_train, 'feat_67')

# plot out all classes
pdp.pdp_plot(pdp_feat_67, 'feat_67', center=True, x_quantile=True, ncols=3)

# only plot out class 2
pdp.pdp_plot(pdp_feat_67, 'feat_67', center=True, multi_flag=True, which_class=2, plot_org_pts=True, plot_lines=True, frac_to_plot=1000, x_quantile=True)
```
----

#### pdpbox.pdp.pdp_interact_plot(pdp_interact_out, feature_names, center=True, plot_org_pts=False, plot_lines=False, frac_to_plot=1., cluster=False, n_cluster_centers=None, cluster_method=None, x_quantile=False, figsize=None, plot_params=None, multi_flag=False, which_class=None, only_inter=False, ncols=None)
  
**Parameters:**
  
* **pdp_interact_out**: instance of pdp_interact_obj  
	- a calculated pdp_interact_obj instance  
  
* **feature_names**: list  
	- a list of two feature names 

**Parameters for pdp plot:**
They are exact the same as those in function **pdpbox.pdp.pdp_isolate_plot**. Now they are applied to two pdp plots.
* **center**: boolean, default=True  
	- whether to center the plot

* **plot_org_pts**: boolean, default=False
	- whether to plot out the original points

* **plot_lines**: boolean, default=False
	- whether to plot out the individual lines

* **frac_to_plot**: float or integer, default=1
	- how many points or lines to plot, can be a integer or a float

* **cluster**: boolean, default=False
	- whether to cluster the individual lines and only plot out the cluster centers

* **n_cluster_centers**: integer, default=None
	- number of cluster centers

* **cluster_method**: string, default=None
	- cluster method to use, default is KMeans, if 'approx' is passed, MiniBatchKMeans is used

* **x_quantile**: boolean, default=False
	- whether to construct x axis ticks using quantiles

* **figsize**: (width, height), default=None
	- figure size

* **plot_params**: dict, default=None
	- values of plot parameters 
	When it comes to interaction plot, user can customize both pdp plots and interaction plot, thus **plot_params** here contains two level. On the top level, you can pass the ploting paramters to interaction plot through 'pdp_inter' key and pass parameters to pdp plot through 'pdp' key.
	
	**under 'pdp_inter' key:**
	- **'font_family'**: font_family for the plot, default='Arial
	- **'title'**: title for the plot, default=ICEplot for (feature_name)
	- **'title_fontsize'**: title font size, default=15
	- **'subtitle_fontsize'**: subtitle font size, default=12
	- **'contour_color'**: color of contour, default='white'
	- **'contour_label_fontsize'**: contour label font size, default=9
	- **'contour_cmap'**: matplotlib color map for contour color fill, default='viridis'

	**under 'pdp' key:**
	- **'pdp_color'**: color for PDP line, default='#1A4E5D'
	- **'pdp_hl_color'**: color for PDP line highlight, default='#FEDC00'
	- **'pdp_linewidth'**: width of PDP line, default=2
	- **'zero_color'**: color of the zero line, default='#E75438'
	- **'zero_linewidth'**: width of the zero line, default=1.5
	- **'fill_color'**: color of the standard deviation fill, default='#66C2D7'
	- **'fill_alpha'**: alpha of the standard deviation fill, default=0.2
	- **'markersize'**: size of the PDP marker, default=5
	- **'point_size'**: size of the original points, default=50
	- **'point_pos_color'**: color of the positive point, default='#5BB573'
	- **'point_neg_color'**: color of the negative point, default='#E75438'
	- **'line_cmap'**: matplotlib color map for individual lines, default='Blues'
	- **'cluster_cmap'**: matplotlib color map for clustered lines, default='Blues'
	
	- Example:
	```python
	plot_params = {
		'pdp_inter': {...} #plot parameters for interaction plot
		'pdp': {...} #same as the one in function pdp_plot
	}
	```

* **multi_flag**: boolean, default=False
	- whether it is a subplot of a multiclass plot

* **which_class**: integer, default=None
	- which class to plot
	
* **only_inter**: boolean, default=False
	- whether only plot the contour interaction plot

* **ncols**: integer, default=None
	- used under multiclass mode when only plot the contour interaction plot
  
**Examples:**
```python
from pdpbox import pdp

inter1 = pdp.pdp_interact(clf, titanic[features], ['Age', 'Fare'])
# plot the complete plot
pdp.pdp_interact_plot(inter1, ['age', 'fare'], center=True, plot_org_pts=True, plot_lines=True, frac_to_plot=0.5)

# only plot the contour interaction plot
pdp.pdp_interact_plot(inter1, ['age', 'fare'], x_quantile=True, only_inter=True)

# for multiclass problem
pdp_67_24 = pdp.pdp_interact(clf, otto_train, ['feat_67', 'feat_24'], num_grid_points=[10, 10])
# only plot out class 5
pdp.pdp_interact_plot(pdp_67_24, ['feat_67', 'feat_24'], center=True, plot_org_pts=True, plot_lines=True, frac_to_plot=0.01, multi_flag=True, which_class=5, x_quantile=True)

# plot contour plots for all classes
pdp.pdp_interact_plot(pdp_67_24, ['feat_67', 'feat_24'], center=True, plot_org_pts=True, plot_lines=True, frac_to_plot=0.01, multi_flag=False, which_class=5, x_quantile=True, only_inter=True, ncols=3)
```