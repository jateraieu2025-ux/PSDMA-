# PCA and Clustering Analysis of UN Country Data

## Project overview

This notebook explores whether countries and territories can be grouped into meaningful development profiles using socioeconomic indicators from `UN.csv`. It combines **Principal Component Analysis (PCA)** for dimensionality reduction with **K-Means clustering** to identify four groups.

**Research question:** Can PCA and clustering reveal meaningful groups of countries based on health, education, fertility, and economic indicators?

## Files required

- `UN_PCA_Clustering_Four_Clusters.ipynb` — the analysis notebook.
- `UN.csv` — the dataset used by the notebook.

Keep the CSV in the notebook's working directory, or update the file path in the data-loading cell.

## Dataset and variables

The analysis uses 207 countries and territories and 12 numerical socioeconomic indicators. The country-name field is kept for identifying countries in the results, but is not used as a numerical PCA feature.

The indicators cover areas such as life expectancy, fertility, infant mortality, income, education, literacy, contraception, and economic activity. Education variables have substantial missingness: `educationMale` and `educationFemale` each have 131 missing values out of 207, so conclusions involving education should be interpreted cautiously.

## Workflow

1. **Load the data** — read `UN.csv` and inspect the dataset.
2. **Handle missing values** — replace missing numerical values with the median using `SimpleImputer(strategy="median")`.
3. **Check possible outliers** — use the interquartile range (IQR) to flag potential outliers. These observations are retained because they may represent real differences between countries.
4. **Standardize the variables** — apply `StandardScaler` so variables measured on different scales can contribute to the analysis more comparably.
5. **Run PCA** — transform the standardized indicators into principal components and examine explained variance and component loadings.
6. **Choose the number of components** — inspect cumulative explained variance.
7. **Evaluate clustering choices** — compare K-Means solutions for different values of *k* using inertia and silhouette scores.
8. **Fit four clusters** — use the selected four-cluster solution to create country groups and describe their profiles.
9. **Interpret the results** — use cluster summaries and representative countries to explain the groups.

## Main PCA results

The first principal component (PC1) explains **53.19%** of the variance and is mainly associated with life expectancy in contrast to fertility, infant mortality, and illiteracy. PC2 explains **13.10%** and is strongly associated with the education indicators. The first three components explain **75.64%** of the variance, while the first six explain **93.63%**. Therefore, six components are retained for the clustering workflow to preserve more than 90% of the total variance.

Component signs can be reversed without changing the underlying interpretation of a PCA axis; focus on which variables contribute together and in contrast to others.

## Clustering and selection of four groups

K-Means solutions were evaluated for values of *k* from 2 to 10.

| Number of clusters (*k*) | Inertia | Silhouette score |
|---:|---:|---:|
| 2 | 1363.679 | 0.398 |
| 3 | 1088.842 | 0.336 |
| 4 | 932.747 | 0.343 |
| 5 | 800.345 | 0.285 |
| 6 | 713.212 | 0.282 |
| 7 | 635.091 | 0.250 |
| 8 | 595.267 | 0.252 |
| 9 | 560.486 | 0.257 |
| 10 | 529.122 | 0.245 |

The silhouette score is highest at *k* = 2, and the elbow pattern does not identify an unequivocal best value. **Four clusters are used for this project to provide a more detailed and interpretable country classification.** This is a deliberate analytical choice, not a claim that four clusters are optimal according to every metric. The four-cluster silhouette score (0.343) is lower than the two-cluster score (0.398), indicating a trade-off between detail and cluster separation.

## Four-cluster interpretation

The cluster numbers are arbitrary labels, not rankings.

- **Cluster 0 — broad middle/mixed profile**: 121 countries; Albania is the representative country closest to the cluster centre. Overall, this group has moderately higher life expectancy and lower fertility, infant mortality, and illiteracy than the dataset average.
- **Cluster 1 — greater development challenges**: 52 countries; Comoros is the representative country. The group tends to have higher fertility, infant mortality, and illiteracy, alongside lower life expectancy and contraception use.
- **Cluster 2 — higher-income/high-human-development profile**: 26 countries; Austria is the representative country. This group has relatively high GDP per capita and education indicators, high life expectancy, and low fertility and infant mortality.
- **Cluster 3 — small, distinct profile**: 8 countries; Laos is the representative country. The group tends to show higher fertility and infant mortality and lower life expectancy. Education-related results require particular caution because the education variables have many missing observations and median imputation can affect the apparent profile.

Representative countries are examples of each group, not evidence that every country in a cluster has identical characteristics.

## How to run the notebook

1. Put the notebook and `UN.csv` in the same folder.
2. Open the notebook in Jupyter Notebook, JupyterLab, or a compatible environment.
3. Ensure the required Python packages are installed: `pandas`, `numpy`, `matplotlib`, `seaborn`, and `scikit-learn`.
4. Run the notebook cells from top to bottom.
5. Review the generated PCA variance plots, component loadings, clustering evaluation plots, and cluster summaries.

If a package is missing, install it in the Python environment used by the notebook, for example with `pip install pandas numpy matplotlib seaborn scikit-learn`.

## Limitations

- Median imputation allows the analysis to run but does not recover the true missing values.
- The large amount of missing data in the two education variables makes interpretations involving education less certain.
- Outliers are retained; some countries may strongly influence PCA components and cluster centres.
- K-Means favours compact groups and may not capture every possible shape of country groupings.
- The four-cluster solution is selected for interpretability even though the silhouette score favours two clusters.
- Results depend on the chosen variables, preprocessing, component count, and clustering method; clusters should be interpreted as exploratory profiles rather than definitive country rankings.

## Conclusion

PCA reduces the 12 standardized indicators to a smaller set of components while retaining most of their variance. K-Means then provides a four-group summary of different socioeconomic profiles, ranging from a relatively high-income and high-life-expectancy group to groups facing greater health and development challenges. The analysis is useful for exploratory comparison, but missing education data and the trade-off between cluster detail and separation should be kept in mind.
