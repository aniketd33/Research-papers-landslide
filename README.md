# Research-papers-landslide

10 real research papers on landslide susceptibility. Each one matches at least two of my project objectives, and together they cover four of my five (the dataset, the model comparison, the validation metrics and SHAP with risk zones). Most Western Ghats papers use random train-test splits. Only one uses spatial cross-validation, which is what my project does, so that is the gap my work fills."

#Literature Review: Landslide Risk Prediction for the Western Ghats of Maharashtra

1. Introduction
Landslide susceptibility mapping predicts which slopes are most likely to fail, so that limited monitoring and mitigation resources go to the highest-risk areas first. The Western Ghats and Konkan belt of Maharashtra see landslides almost every monsoon, and recent regional studies use machine learning on terrain, rainfall, land cover and proximity features to map this risk.
This review covers ten studies published between 2020 and 2026. Five are from the Western Ghats (Raigad and Western Maharashtra, Goa, Chikmagalur in Karnataka, and Kozhikode in Kerala) [1]–[5]. The other five, from Peru, Italy, China and Egypt, were chosen because each addresses one specific objective of this project: validation, evaluation metrics, non-landslide sampling, model comparison and explainability [6]–[10].
The review is organised around the project objectives: building the dataset, comparing models, validating and measuring them, explaining the results with SHAP, and classifying areas into risk zones. It ends with the research gap that this project addresses.
2. Landslide susceptibility studies in Maharashtra and the Western Ghats
In the Western Ghats studies that compared several models, tree-based ensembles gave the best scores, and slope is among the most influential factors in four of the five studies.
• Pawar and Sharma [1] mapped Raigad district using 174 landslides (1998–2023) taken from the literature, the NASA Landslide Viewer and Bhukosh (GSI). Random Forest (AUC 0.90, F1 0.79) beat SVM (0.81), Frequency Ratio (0.79) and Shannon Entropy (0.80). SHAP and partial dependence plots both ranked slope first.
• Pawar and Aravinth [2] studied eight talukas of Western Maharashtra, including Mahad and Poladpur, with Random Forest, XGBoost and a neural network on 13 factors. XGBoost reached the highest accuracy (91.17%), ahead of Random Forest (85.29%) and the neural network (82.35%).
• Ganesh et al. [3] mapped Goa using 78 GSI landslide points and 200 random non-landslide points. Random Forest had the best AUC (0.86), followed by Logistic Regression (0.84), KNN (0.83) and SVM (0.81). Distance to road, slope and annual rainfall ranked highest.
• Ganesh et al. [4] compared Decision Tree, Random Forest, XGBoost and CatBoost for Chikmagalur using 197 GSI events and 20 factors. CatBoost was best (AUC 0.95, F1 0.88) with Random Forest close behind. Slope, rainfall, soil type and road distance were the top factors.
• Badapalli et al. [5] applied Random Forest to Kozhikode, Kerala, with 231 GSI points and an equal number of random non-landslide points. The model reached an AUC of about 0.89 and placed 17.8% of the district in the high or very high classes.
Two limits recur. None of the five studies uses spatial cross-validation, and rainfall is weak: [1] uses a single year (2023) against an inventory spanning 1998–2023, and [5] has no rainfall layer in its model.
3. Building the dataset: inventory and non-landslide sampling
Studies in India build their inventory from Geological Survey of India (GSI) records, and the choice of non-landslide points varies widely and affects the results.
• Inventory. [1], [3], [4] and [5] use GSI/Bhukosh landslide points, alone or combined with other sources. [2] uses 345 of 482 landslide points but does not name the source.
• Random non-landslide points. [3] adds 200 random points to 78 landslides, and [5] adds an equal number of random points to 231 landslides.
• Stratified sampling away from known slides. [1] draws 174 non-landslide points (1:1) from stable terrain, outside the influence zones of known landslides.
• Comparison of sampling methods. Tu et al. [8] compared six strategies in Yongxin County, China (random, 1,000 m buffer, rule-based grid, Frequency Ratio, Information Value and Certainty Factor) with 333 landslides and Random Forest. Sample selection changed performance substantially, so scores from different sampling schemes should not be compared directly.
• Conditioning factors. Rainfall, land cover, and distance to roads or rivers are common in [1], [3] and [4], alongside slope, elevation and aspect. [1] takes rainfall from IMD gridded data for 2023 only.
This project follows the same pattern. It uses 2,544 points (1,272 landslide and 1,272 non-landslide), with non-landslide points drawn at random but at least 1 km from any recorded landslide, similar in spirit to the buffer and influence-zone approaches in [1] and [8].
4. Machine learning models compared in earlier studies
Random Forest is the most widely used model, and Logistic Regression is a strong, close baseline, so comparing the two with a boosted or single-tree model is the usual design.
• Logistic Regression as baseline. In [3] it scored an AUC of 0.84 against 0.86 for Random Forest. In [9] (Egypt, 183 landslide and 183 non-landslide points) it scored 0.88 against 0.95 for Random Forest and 0.90 for SVM.
• Random Forest. It was the best model in [1], [3] and [9], and the only model in [5].
• Boosted models. XGBoost was best in [2], CatBoost was best among four tree-based models in [4], and XGBoost is the model explained in [10].
• Imbalance handling. [2] used SMOTE to balance its classes. [3], [5], [8] and [9] avoided imbalance by sampling equal numbers of landslide and non-landslide points.
• Wider model sets. Kumar et al. [6] compared seven models (Logistic Regression, LDA, KNN, ANN, SVM, Random Forest and C5.0) on 1,460 landslides in Arequipa, Peru, using 24 factors.
These results justify a Logistic Regression baseline with tree-based models for this project.
5. Validation and evaluation metrics
Most Western Ghats studies validate with random splits, while recent work argues that random cross-validation gives a biased assessment and that precision-recall measures suit this problem better than ROC alone.
• Random splits are the norm. [1] uses 10-fold cross-validation with a 70/30 split, [3] a random 75/25 split, [4] 5-fold stratified cross-validation, [9] a 70:30 split, and [2] a 90/5/5 split. None of these keeps neighbouring locations apart.
• Spatial cross-validation. Kumar et al. [6] compare random and spatial cross-validation on the same data. Their spatial folds come from k-means clustering of the sample coordinates, with 10 folds and five repetitions, and their title states the conclusion: random cross-validation produces a biased assessment of performance.
• Choice of metric. Yordanov and Brovelli [7] compare ROC and precision-recall curves for Statistical Index, Logistic Regression and Random Forest in Val Tartano, Italy. The two behave similarly on balanced data, but the precision-recall curve describes performance more precisely when classes are imbalanced, and ROC has been criticised for being unreliable in that case.
• Metrics reported in practice. The Western Ghats studies report AUC-ROC, accuracy, precision, recall and F1 [1]–[4]. None of the five reports PR-AUC.
This supports the choice of spatial block cross-validation and a metric set of AUC-ROC, PR-AUC and F1 for this project.
6. Explainability (SHAP) and susceptibility zoning
SHAP is increasingly used to show which factors drive a landslide model, and most studies group the output into five risk classes.
• SHAP in Maharashtra. [1] applied SHAP to Random Forest and SVM, and added partial dependence plots for Random Forest. Slope was the top predictor in both. In contrast, [2] notes that its Random Forest, XGBoost and neural network models lack transparency, which limits interpretation of individual factors.
• SHAP elsewhere. [9] used SHAP on Logistic Regression, Random Forest and SVM, and found slope the most influential factor in all three. [8] used SHAP across its six sampling methods, and NDVI, slope, lithology, land cover and elevation contributed most. Zhang et al. [10] explain an XGBoost model with SHAP at global and local levels and apply it to regions with different topography, geology and vegetation to test how well the findings generalise.
• Zoning. [1], [3] and [5] classify areas into five levels (very low to very high). [4] reports that 20.53% of Chikmagalur is highly susceptible and 47.04% moderately susceptible.
This project uses three classes, Low (score below 0.4), Moderate (0.4 to 0.7) and High (0.7 or above), which are simpler to read on a map and to use for planning.
7. Summary of the 10 papers mapped to project objectives
Together the ten papers cover objectives O2 to O5, and each paper matches at least one objective.
#
Paper
Region
Models
Validation
Objectives matched
1
Pawar & Sharma (2026)
Raigad, Maharashtra
Random Forest, SVM, Frequency Ratio, Shannon Entropy
10-fold CV, 70/30 split
O2, O3, O5; partly O4
2
Pawar & Aravinth (2025)
Western Maharashtra (8 talukas)
Random Forest, XGBoost, neural network
90/5/5 split, SMOTE
O3; O5 (zones only)
3
Ganesh et al. (2023)
Goa
Logistic Regression, SVM, KNN, Random Forest
Random 75/25 split
O2, O3; partly O4
4
Ganesh et al. (2026)
Chikmagalur, Karnataka
Decision Tree, Random Forest, XGBoost, CatBoost
5-fold stratified CV
O3, O5 (zones); partly O4
5
Badapalli et al. (2025)
Kozhikode, Kerala
Random Forest
70/30 split
O2; O5 (zones)
6
Kumar et al. (2025)
Arequipa, Peru
7 models incl. Logistic Regression, Random Forest
Random vs spatial CV
O4
7
Yordanov & Brovelli (2020)
Val Tartano, Italy
Statistical Index, Logistic Regression, Random Forest
ROC vs precision-recall
O4
8
Tu et al. (2025)
Yongxin, China
Random Forest
7:3 split, six sampling methods
O2, O5
9
Abdelkader & Csámer (2025)
Cairo–Suez, Egypt
Logistic Regression, Random Forest, SVM
70:30 split
O3, O5; partly O4
10
Zhang et al. (2023)
Mountainous hilly regions, China
XGBoost
Cross-region test
O5
Objectives: O1 analysis of the landslide inventory by district, material and movement type; O2 landslide vs non-landslide dataset with terrain, rainfall, land cover and proximity features; O3 comparison of Logistic Regression, Random Forest and XGBoost; O4 spatially aware validation with AUC-ROC, PR-AUC and F1; O5 SHAP and Low/Moderate/High zones.
O1 is not the main focus of any of the ten papers. [1], [3], [4] and [5] do build their inventories from GSI/Bhukosh records, so they support the data source.
8. Research gap and how this project addresses it
The reviewed Western Ghats studies produce good-looking maps, but their reported scores come from validation that does not test the models on genuinely new areas.
1.  Spatial validation is missing in the region. Only [6], from Peru, uses spatial cross-validation. The five Western Ghats studies [1]–[5] use random or stratified splits, which can let neighbouring locations fall in both training and test sets.
2.  Metrics are narrow. None of the Western Ghats studies reports PR-AUC, even though [7] shows it describes performance better than ROC when classes are imbalanced.
3.  Rainfall data is weak. [1] uses one year of rainfall (2023) against an inventory spanning 1998–2023, and [5] has no rainfall layer. Landslide records without dates cannot support true forecasting.
4.  Inventory profiling is rare. None of the ten studies analyses the inventory by district, material and movement type as a main objective.
What this project does. It builds a balanced dataset of 2,544 points (1,272 landslide and 1,272 non-landslide, the latter at least 1 km from any recorded landslide). It compares Logistic Regression, Decision Tree and Random Forest using 5-fold spatial GroupKFold cross-validation on 0.1-degree map blocks, so every place is tested by a model that never saw its block. Random Forest performs best, with an AUC-ROC of 0.832 and an F1 of 0.767.
This AUC is lower than the 0.86 to 0.95 reported in [1], [3], [4], [5] and [9], which use random splits. The datasets differ, so the numbers are not directly comparable, but the gap is consistent with the concern in [6] that random cross-validation gives a biased assessment.
Slope is also the strongest factor here (importance 0.48), matching [1], [3], [4], [5] and [9]. A rainfall what-if test shows only a small, uneven effect, which the project attributes to having one rainfall value per area, the same weakness noted in [1] and [5]. Results are shown as a Low/Moderate/High risk map with a hotspot list in a Streamlit dashboard, framed as decision support and not as a forecast of when a landslide will occur.
References
1.  N. S. Pawar and K. V. Sharma, "Interpretable Landslide Hazard Analysis in the Western Ghats of Maharashtra, India: A Hybrid Machine Learning and Statistical Approach," Geological Journal, 2026, pp. 1–22. doi:10.1002/gj.70175
2.  S. V. Pawar and Aravinth R., "Geospatial Machine Learning Models for Integrated Landslide Risk and Asset Exposure Assessment," ISPRS Annals of the Photogrammetry, Remote Sensing and Spatial Information Sciences, vol. X-5/W2-2025, 2025. doi:10.5194/isprs-annals-X-5-W2-2025-511-2025
3.  B. Ganesh, S. Vincent, S. Pathan and S. R. Garcia Benitez, "Integration of GIS and Machine Learning Techniques for Mapping the Landslide-Prone Areas in the State of Goa, India," Journal of the Indian Society of Remote Sensing, vol. 51, no. 7, pp. 1479–1491, 2023. doi:10.1007/s12524-023-01707-y
4.  B. Ganesh, S. Vincent, S. Pathan and G. V. Bhat, "Landslide susceptibility mapping using tree-based machine learning classifiers and remote sensing derived conditioning factors: A case study of Chikmagalur District, Western Ghats, India," Egyptian Journal of Remote Sensing and Space Science, vol. 29, no. 1, pp. 209–222, 2026. doi:10.1016/j.ejrs.2026.02.004
5.  P. K. Badapalli et al., "Landslide Susceptibility Level Mapping in Kozhikode, Kerala, Using Machine Learning-Based Random Forest, Remote Sensing, and GIS Techniques," Land, vol. 14, no. 7, art. 1453, 2025. doi:10.3390/land14071453
6.  C. Kumar, G. Walton, P. Santi and C. Luza, "Random Cross-Validation Produces Biased Assessment of Machine Learning Performance in Regional Landslide Susceptibility Prediction," Remote Sensing, vol. 17, no. 2, art. 213, 2025. doi:10.3390/rs17020213
7.  V. Yordanov and M. A. Brovelli, "Comparing Model Performance Metrics for Landslide Susceptibility Mapping," International Archives of the Photogrammetry, Remote Sensing and Spatial Information Sciences, vol. XLIII-B3-2020, pp. 1277–1284, 2020. doi:10.5194/isprs-archives-XLIII-B3-2020-1277-2020
8.  L. Tu et al., "Improving Landslide Susceptibility Assessment Through Non-Landslide Sampling Strategies," Land, vol. 14, no. 10, art. 2059, 2025. doi:10.3390/land14102059
9.  M. M. Abdelkader and Á. Csámer, "Comparative assessment of machine learning models for landslide susceptibility mapping: a focus on validation and accuracy," Natural Hazards, vol. 121, no. 9, pp. 10299–10321, 2025. doi:10.1007/s11069-025-07197-0
10.  J. Zhang et al., "Insights into geospatial heterogeneity of landslide susceptibility based on the SHAP-XGBoost model," Journal of Environmental Management, vol. 332, art. 117357, 2023. doi:10.1016/j.jenvman.2023.117357
