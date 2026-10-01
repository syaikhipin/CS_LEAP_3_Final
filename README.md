# Clustering the Seeds dataset: how much structure can you find without labels?

Final project (Option 3) for Machine Learning with Python (LEAP).

## The problem

An agricultural co-op gets deliveries of mixed wheat grain and suspects several varieties are present, but nothing arrives labelled. Before paying for a sorting process, they want to know how many varieties are plausibly in the mix and how cleanly they separate on a few simple measurements. That is an unsupervised problem, so in this project the true variety labels are put aside at the start. The clustering never sees them. They come back only at the end, as an answer key.

## The data

I used the Seeds dataset from OpenML: 210 wheat kernels, each described by seven geometric measurements (area, perimeter, compactness, kernel length and width, asymmetry coefficient, groove length), drawn from three varieties coded 1, 2 and 3. Because clustering depends on distances, I standardised all seven features with `StandardScaler` first; otherwise area (roughly 10-21) would swamp compactness (roughly 0.8-0.9) just because of its units.

## What I did

1. Reduced the seven features to two with PCA. The two components keep about 89% of the variance. I looked at a single-colour scatter.
2. Ran K-Means for k = 2 to 7 and drew the two diagnostics from the course: inertia for the elbow method, and the silhouette score.
3. Picked k and fitted K-Means (`n_init=10`, `random_state=42`) on the scaled features.
4. Brought the labels back as an external check: cross-tabulation, purity score, the PCA scatter recoloured by true variety, and cluster centres converted back to the original units.

## Results

The elbow sat at k = 3. The silhouette preferred k = 2 (0.47), with k = 3 close behind (0.40), so the two diagnostics disagreed. The choice needed an argument rather than a read-off from a single chart; the reasoning is in the notebook. I went with k = 3.

At k = 3 the purity score is 0.919: about 92% of kernels sit in a cluster whose majority matches their true variety. Variety 2 separates almost perfectly (65 of 67 kernels in one cluster). Varieties 1 and 3 are the hard pair, and each leaks kernels into the other's cluster. The recoloured scatter shows the same thing: one clean cloud, and one shared one.

## What I took away

Scaling first and keeping the labels hidden until the end made the exercise honest: every decision about k was made blind, so the checks at the end could actually surprise me, and did. The hardest part was the silhouette disagreement. Two diagnostics pointed to different k values, so I had to defend my choice in writing rather than let a chart decide it. With more time I would try a Gaussian mixture or hierarchical clustering to see whether the 1-vs-3 confusion is specific to K-Means and its spherical-cluster assumption. I would also look at per-point silhouette values to see exactly which kernels sit between clusters, and have an expert physically label a sample from each cluster before anyone acts on the grouping.

## Files

- `Final_Project_Option3_Seeds_Clustering.ipynb`, the completed notebook with all outputs visible (run top to bottom in Google Colab)
- `README.md`, this file
