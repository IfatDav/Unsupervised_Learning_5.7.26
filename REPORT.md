# REPORT — Module 3 · Assignment 2 · Unsupervised Learning

**Name:** Ifat Davidson  **ID:** 037377140  **Date:** 4.7.26
**Chosen option:** A (A · Olist segmentation / B · Credit Card / C · Olist anomaly)

---

## 1. Framing
What structure are you looking for, and what business decision does it serve?
I am seeking latent customer segments based on distinct purchasing patterns to replace broad-brush marketing strategies. This structure serves the business decision of enabling personalized communication, specifically identifying which customer groups respond better to aggressive discount campaigns versus those who prefer loyalty content or premium product recommendations.

Distance / similarity measure chosen, and why:
I chose Cosine Similarity as my primary metric. Since my analysis focuses on relative product category preferences within a basket rather than raw monetary volume, this measure effectively captures the orientation of consumption patterns rather than their absolute magnitude.

Feature choices (and what you did about frequency / missing values):
I focused on behavioral features derived from product category popularity and basket composition, while explicitly excluding recency and frequency metrics as instructed. I applied rigorous scaling to ensure that feature magnitudes do not bias the distance calculations, and I handled missing values by imputing them with zero, as in this context, a null value represents a lack of interaction with a specific product category.

---

## 2. Method & validation
| Item | Value |
|---|---|
| Approaches tried | |
| Chosen k (Elbow vs Silhouette) | |
| Silhouette score | |
| Cluster sizes | |
| Stability across seeds / subsamples | |

---

## 3. Guiding questions (graded)
Answer each in 2-5 sentences.

1. **No ground truth.** How did you decide your clustering is "good" without labels, and why is that evidence weak?
2. **Choosing k.** What did Elbow say vs Silhouette? Where did they disagree, and which did you trust?
3. **Scaling.** How did feature scaling change the clusters? Show a before/after for one decision.
4. **Stability.** Re-run with different seeds / on a subsample. Do the clusters survive? Would you trust them on next month's data?
5. **What defines each cluster.** Name the 2-3 features that separate clusters. Do the personas make business sense?
6. **Real or artifact.** Is any "cluster" just an artifact of the algorithm's assumptions (e.g. KMeans forcing spheres)? How did you check?
7. **Action.** For each segment, one concrete action a marketing / ops team could take. If you can't name one, is the segment useful?
8. **Cost of a false alarm.** (Anomaly option, or one line for clustering.) Why "candidates for investigation" and not "fraud"? What does a false alarm cost?

---

## 4. Structure Card
Paste the completed Structure Card from the notebook here.

```
(Structure Card)
```

---

## 5. Reflection
What surprised you? Would these segments hold on new data? How would this feed your mid-term project?
