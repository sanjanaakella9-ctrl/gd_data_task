# gd_data_task
# Problem Statement: 
# Concrete’s safety and durability is majorly determined by its compressive strength. The strength of concrete also depends on its mixture composition and curing time. The goal of this project is to develop a regression model using exploratory data analysis and ensemble learning techniques (AdaBoost.R2), that predicts the compressive strength of concrete (in MPa) based on its material composition, age and evaluate model performance using R² score, and compare a from-scratch implementation with scikit-learn’s implementation. The dataset consists of 1030 samples containing quantities of cement, slag, fly ash, water, superplasticizer, coarse aggregate, fine aggregate, and curing age.
# Dataset:
The dataset contains quantities of cement, slag, fly ash, water, superplasticizer, coarse aggregate, fine aggregate, and curing age. It is used for supervised learning to determine concrete strength.

Rows: 1030
Features:  Cement, Slag, Fly Ash, Water, Superplasticizer, Coarse Aggregate, Fine Aggregat, and Curing Age
Target Variable: Concrete Compressive Strength (supervised)
Observations from Initial Inspection
No categorical variables are present.


Features are on different scales (e.g., Age ranges up to hundreds, Superplasticizer is much smaller).


Outliers are visible in some components (especially Age and Superplasticizer).


Some features show skewed distributions.

Tools:
Pandas
Numpy 
Matplotlib
Seaborn 
Scikit Learn
Approach and Methodology:
.Data Cleaning:
Cleaned the Data (no duplicate/ missing values found).
Splitting the Data:
The dataset was split into training, validation and test sets (70%-15%-15%) using train_test_split.
It was first split into training and set (temporary) into 70, 30. The set was further split into 50, 50 as validation and test sets (15 each). 
Data Visualisation:
Univariate:
Target Distribution


The compressive strength distribution is slightly right-skewed.


Higher strength values are less frequent.


This suggests the presence of some high-performance concrete mixtures.


Boxplots


Age is highly skewed.


Superplasticizer contains many low values.


Different features operate on very different ranges.


This confirms that mixture composition varies significantly across samples.
Bivariate Insights
Correlation Analysis


Cement shows strong positive correlation with strength.


Water shows negative correlation with strength.


Age shows moderate positive correlation.


Some materials (e.g., Fly Ash) show weak correlation individually.


This suggests:
Higher cement → stronger concrete


Higher water → weaker concrete (dilution effect)


Scatter Observations


Relationships are not strictly linear.


Interaction between water and cement seems important.


Thus, a non-linear model is more appropriate than linear regression.
Multivariate Insights
Correlation heatmap shows:


Some trade-offs between raw materials (e.g., Cement vs Slag).


No severe multicollinearity.


Pairplots show:


Age amplifies the effect of cement.


Non-linear patterns are visible. 

Models used:
Since relationships are non-linear and interaction-based, a tree-based boosting model like AdaBoost.R2 is justified over linear regression.
AdaBoost was chosen because:
The dataset shows non-linear relationships.


Boosting reduces bias of weak learners.


It performs well on structured tabular data.


It allows understanding of ensemble learning mechanics via scratch implementation.
What is Adaboost regressor model:
Instead of building one strong model, adaboost works on the principle of building many weak learners and training them sequentially by considering the poorly predicted points iteratively. 
Mathematical Flow: 
Step 1: Initialize Sample Weights
Given a training dataset with NNN samples:
wi=1N,i=1,2,...,Nw_i = \frac{1}{N}, \quad i = 1, 2, ..., Nwi​=N1​,i=1,2,...,N
All samples are assigned equal importance at the beginning.

Step 2: Boosting Iterations
For each boosting round m=1,2,...,Mm = 1, 2, ..., Mm=1,2,...,M:

2.1 Train Weak Learner
Train a weak regression model hm(x)h_m(x)hm​(x) using the current sample weights wiw_iwi​.
The model is fitted using weighted training, so samples with higher weights influence the model more.

2.2 Compute Normalized Absolute Error
First compute absolute errors:
∣yi−hm(xi)∣|y_i - h_m(x_i)|∣yi​−hm​(xi​)∣
Then normalize errors:
Li=∣yi−hm(xi)∣max⁡(∣y−hm(x)∣)L_i = \frac{|y_i - h_m(x_i)|}{\max(|y - h_m(x)|)}Li​=max(∣y−hm​(x)∣)∣yi​−hm​(xi​)∣​
This ensures:
0≤Li≤10 \leq L_i \leq 10≤Li​≤1
Normalization prevents extreme error values from dominating weight updates.

2.3 Compute Weighted Error
em=∑i=1NwiLie_m = \sum_{i=1}^{N} w_i L_iem​=i=1∑N​wi​Li​
This represents the overall performance of the weak learner.
If:
em≥0.5e_m \geq 0.5em​≥0.5
The model is discarded, as it performs worse than a weak baseline.

2.4 Compute Model Weight
First compute:
βm=em1−em\beta_m = \frac{e_m}{1 - e_m}βm​=1−em​em​​
Then compute model importance:
αm=ln⁡(1βm)\alpha_m = \ln\left(\frac{1}{\beta_m}\right)αm​=ln(βm​1​)
Interpretation:
Lower error → smaller βm\beta_mβm​


Smaller βm\beta_mβm​ → larger αm\alpha_mαm​


Better models get higher influence in final prediction



2.5 Update Sample Weights
wi=wi⋅βm(1−Li)w_i = w_i \cdot \beta_m^{(1 - L_i)}wi​=wi​⋅βm(1−Li​)​
Interpretation:
If LiL_iLi​ is large (poor prediction) → weight increases


If LiL_iLi​ is small (good prediction) → weight decreases


After updating:
wi=wi∑wiw_i = \frac{w_i}{\sum w_i}wi​=∑wi​wi​​
Weights are normalized to sum to 1.

Step 3: Final Prediction
The final regression prediction is a weighted average of all weak learners:
F(x)=∑m=1Mαmhm(x)∑m=1MαmF(x) = \frac{\sum_{m=1}^{M} \alpha_m h_m(x)}{\sum_{m=1}^{M} \alpha_m}F(x)=∑m=1M​αm​∑m=1M​αm​hm​(x)​
Each weak learner contributes according to its performance weight αm\alpha_mαm​.




Evaluation Metrics
Since this is a regression problem, clustering metrics like WCSS or Inertia are not applicable. Instead, the following regression metrics were used:
1. R² Score
R² measures how well the model explains the variation in compressive strength.
 Higher R² means better performance.
2. Mean Squared Error (MSE)
It measures the average squared difference between actual and predicted strength values.
 Lower MSE indicates better accuracy.
3. Mean Absolute Error (MAE)
It measures the average absolute difference between actual and predicted values.
 It is easy to interpret since it is in MPa units.
Residual plots were also observed to check whether errors are randomly distributed.


Insights
Step 2 – Data Understanding (EDA Insights)
The compressive strength distribution is slightly right-skewed.


Most strength values lie between 20–50 MPa.


Age is highly skewed.


Superplasticizer has many low or zero values.


Features are on very different scales.


This shows that strength depends on multiple interacting factors and not just one feature.

Step 4 – Model Understanding
AdaBoost.R2 builds multiple small trees.


Each new model focuses more on the samples that were predicted poorly earlier.


Sample weights keep changing after every iteration.


This helps the model reduce error step by step.


This makes boosting suitable for capturing non-linear relationships in the dataset.
Step 10 – Final Insights & Conclusion
Concrete strength depends on mixture composition and curing age.


The relationship is non-linear.


Boosting performed better than a single weak learner.


The scratch implementation gave similar results to scikit-learn, confirming correctness.


Tree-based models worked well even though feature scales were very different.


Overall, AdaBoost.R2 was effective for predicting compressive strength and handled complex interactions between features well.
How to run the project:
Download or clone the repository.
Open the project folder.
Open the Jupyter Notebook file (.ipynb).
Run all the cells from top to bottom.

Project Structure: 
├── Concrete_data.xls        # Dataset used in the project
├── gc.ipynb      # Jupyter Notebook with full code
├── README.md          # Project explanation
