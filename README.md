# HAM-SyntheticDarker and HAM-HybridEquity: Two datasets designed to reduce bias in melanoma diagnosis

## HAM-SyntheticDarker

A critical question emerges, after noting that authentic diversity is extraordinarily scarce in literature: what exactly is lost when training data lacks diverse representation? The dataset HAM-SyntheticDarker approaches this problem and tries to solve it through controlled and systematic transformation. Considering that attempting to manually curate authentic images is an impractical approach, as demonstrated by Pipsqueak, we employ systematic transformation to create an exact converted dataset where each HAM10000 image, related to melanoma or nevus class, has undergone a process of brightness and color matching with respect to darker skin types. Each image has undergone a process of histogram matching in LAB color space compared to an image chosen empirically as representative of skin types V and VI on the Fitzpatrick scale, with the aim of creating a paired dataset where lesion morphology remains identical, while photometric characteristics are fundamentally altered.

## HAM-HybridEquity

This final dataset represents a pragmatic hypothesis: given that authentic diverse data collection is time-intensive and resource-constrained, can we improve model robustness in the interim through combined training on original and synthetically-diversified data? HAM-HybridEquity merges the original HAM10000 subsets with their HAM-SyntheticDarker counterpart, creating a dataset of manufactured diversity. This is not presented as a solution but as an experiment that acknowledges both the utility and limitations of synthetic approaches. The results demonstrate that synthetic diversity reduces performance variance across domains but at the cost of reduced in-domain accuracy, and with substantial persistent gaps relative to authentic diversity. It embodies a necessary humility: when faced with systemic data bias, we can explore interim strategies while remaining committed to the fundamental imperative of authentic diverse data collection.

# How to download the datasets
The datasets are publicly available in .npy format at the following link:

https://drive.google.com/drive/folders/1oL_LeZlGNBdYNJe_DiKvkclHZZ2clCzA?usp=drive_link

# How to Cite

The datasets are freely available for use. When using them, please cite the original publications. When referencing this dataset in your own manuscripts and publications, please use the following full citations:

Ruga, T., Zumpano, E., Vocaturo, E., & Caroprese, L. (2026). Reasoning on the gap between synthetic and authentic diversity: The limits of computational solutions to representation bias. *Journal of Computational Science*, Article 102849. [https://doi.org/10.1016/j.jocs.2026.102849](https://doi.org/10.1016/j.jocs.2026.102849)

As likewise applies to the original study:

Ruga, T., Zumpano, E., Vocaturo, E., Caroprese, L., & Arlia, C. (2025, July). Bias in Dermatological Datasets: A Critical Analysis of the Underrepresentation of Dark Skin Tones in Melanoma Classification Images. In International Conference on Computational Science (pp. 434-448). Cham: Springer Nature Switzerland.

# How to load datasets and create subsets

To be able to emulate the configurations used in the experiments proposed in the original paper, after loading the datasets, refer to the following code to obtain the exact distributions:

```python
if DATASET == 'HAM10000':
  x_train,x_test,y_train,y_test = train_test_split(X,y,test_size=0.3,random_state=11,stratify=y)
  x_val,x_test,y_val,y_test = train_test_split(x_test,y_test,test_size=0.3,random_state=11,stratify=y_test)
elif DATASET == 'HAM-CONV':
  x_train,x_test,y_train,y_test = train_test_split(X_conv,y_conv,test_size=0.3,random_state=11,stratify=y_conv)
  x_val,x_test,y_val,y_test = train_test_split(x_test,y_test,test_size=0.3,random_state=11,stratify=y_test)
elif DATASET == 'HAM-BALANCED':
  x_train,x_test,y_train,y_test = train_test_split(X,y,test_size=0.3,random_state=11,stratify=y)
  x_val,x_test,y_val,y_test = train_test_split(x_test,y_test,test_size=0.3,random_state=11,stratify=y_test)

  x_train_conv,x_test_conv,y_train_conv,y_test_conv = train_test_split(X_conv,y_conv,test_size=0.3,random_state=11,stratify=y_conv)
  x_val_conv,x_test_conv,y_val_conv,y_test_conv = train_test_split(x_test_conv,y_test_conv,test_size=0.3,random_state=11,stratify=y_test_conv)

  x_train = np.concatenate([x_train, x_train_conv], axis=0)
  y_train = np.concatenate([y_train, y_train_conv], axis=0)

  x_val = np.concatenate([x_val, x_val_conv], axis=0)
  y_val = np.concatenate([y_val, y_val_conv], axis=0)

  x_test = np.concatenate([x_test, x_test_conv], axis=0)
  y_test = np.concatenate([y_test, y_test_conv], axis=0)

```

  To obtain HAM-HybridEquity, which is created by merging the two datasets available in the repository, refer to the same code mentioned above.
