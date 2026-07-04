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
| Approaches tried | KMeans (Main approach) and DBSCAN (Secondary approach). |
| Chosen k (Elbow vs Silhouette) | 4. The Elbow curve was almost linear, and the Silhouette score peaked at 2. I chose 4 because 2 segments provide no actionable marketing value, and 4 aligns with a local peak/recovery in the Silhouette plot. |
| Silhouette score | ~0.0 (Close to zero, reflecting highly overlapping, noisy behavioral data). |
| Cluster sizes | C0: 93.6%, C1: 0.04%, C2: 3.5%, C3: 3.2%. |
| Stability across seeds / subsamples | Highly stable. Re-running KMeans with seeds 42, 123, and 999 consistently produced one massive cluster (91-96%) and three tiny outlier clusters. |

---

## 3. Guiding questions (graded)
Answer each in 2-5 sentences.

1. **No ground truth.** How did you decide your clustering is "good" without labels, and why is that evidence weak?
I evaluated the clustering using a combination of internal mathematical metrics (Elbow and Silhouette scores) and stability checks across different random seeds. However, this evidence is inherently weak because these internal metrics strictly measure geometric tightness and mathematical separation. A mathematically "perfect" sphere does not guarantee that the resulting clusters represent coherent, logical, or actionable business personas in the real world.
   
2. **Choosing k.** What did Elbow say vs Silhouette? Where did they disagree, and which did you trust?
The Elbow curve was almost perfectly linear without a distinct "knee", offering little guidance, while the Silhouette score peaked at $k=2$ but quickly dropped to negative values. The metrics disagreed because $k=2$ mathematically minimized overlap, but it provided zero actionable marketing value. I ultimately trusted a local peak/recovery in the Silhouette plot at $k=4$, prioritizing the business need for multiple distinct personas over pure mathematical separation.
   
3. **Scaling.** How did feature scaling change the clusters? Show a before/after for one decision.
Distance-based algorithms like KMeans are heavily biased by absolute magnitudes, so feature scaling using StandardScaler was crucial to normalize variances. Before scaling, high-frequency categories like cama_mesa_banho dominated the variance (0.166 vs. others), artificially forcing clusters to be defined solely by the most popular items. After scaling, all features were normalized to a variance of 1.0, allowing the algorithm to detect underlying relative preferences rather than just raw purchase volumes.
  
4. **Stability.** Re-run with different seeds / on a subsample. Do the clusters survive? Would you trust them on next month's data?
I tested stability by re-running the KMeans model with three different random seeds (42, 123, 999). The clusters survived consistently, always returning one massive baseline segment (91-96%) and three tiny outlier segments. While technically stable, I would be hesitant to trust them for strategic marketing on next month's data, as the model learned to isolate extreme, highly specific niches rather than finding broad, repeatable behavioral patterns among the general customer base.
   
5. **What defines each cluster.** Name the 2-3 features that separate clusters. Do the personas make business sense?
Cluster 0 (~94%) acts as a baseline with no strong defining features, representing the average one-off buyer. The remaining clusters are separated by single, high-variance categories: Cluster 1 is defined by moveis_colchao_e_estofado (Furniture buyers), Cluster 2 by perfumaria and beleza_saude (Beauty shoppers), and Cluster 3 by bebes and brinquedos (New parents). While the small niche personas make intuitive sense, the fact that 94% of users fall into a single, generic "average" persona severely limits the overall business usefulness of the segmentation.
   
6. **Real or artifact.** Is any "cluster" just an artifact of the algorithm's assumptions (e.g. KMeans forcing spheres)? How did you check?
Yes, these clusters are primarily mathematical artifacts caused by KMeans attempting to impose spherical boundaries on highly sparse behavioral data. I verified this through a 2D PCA visualization, which explained a dismal ~3.0% of the variance and displayed an inseparable cloud rather than distinct groups. The algorithm essentially acted as a crude anomaly detector, dumping 94% of users into a central "average" mass and isolating single-category edge cases into separate "clusters."

7. **Action.** For each segment, one concrete action a marketing / ops team could take. If you can't name one, is the segment useful?
For Cluster 0 (Average Buyer), the action is to avoid high-cost retention and use generic automated discounts. For Cluster 1 (Furniture), trigger cross-sell emails for home decor two weeks post-delivery. Cluster 2 (Perfume) should receive a replenishment discount after 3 months, and Cluster 3 (New Parent) requires an age-progressive funnel promoting toddler toys 6 months later. While actions exist for the extreme niches, the massive baseline cluster is barely actionable, making the overarching structure quite weak for targeted marketing.


8. **Cost of a false alarm.** (Anomaly option, or one line for clustering.) Why "candidates for investigation" and not "fraud"? What does a false alarm cost?
Although I selected the clustering option, the anomaly concept applies to the tiny outlier clusters: they are statistical "candidates for investigation" or niche targeting, rather than definitive segments. The cost of a false alarm in this context is relatively low—perhaps sending a slightly irrelevant email promotion. However, continuously misclassifying generic buyers as high-value niche customers wastes marketing budget and risks user fatigue from mismatched campaigns.
---

## 4. Structure Card
Paste the completed Structure Card from the notebook here.

```
# Structure Card

## 1. Overview
# Structure Card
- **Option and data:** Option A (Olist Customer Segmentation).
- **Features used and why:** I used behavioral features based on product category preferences (e.g., counts per category). I explicitly excluded the traditional 'Frequency' metric because ~97% of Olist customers only ever make a single purchase, making it useless for segmentation. Missing values were imputed with 0, as a null simply means the customer did not buy from that category. All features were normalized using `StandardScaler` to prevent high-volume categories from dominating the distance calculations.

## 2. Method & validation
- **Approaches tried, and chosen k:** I experimented with KMeans and DBSCAN. For KMeans, I chose k=4. The Elbow curve was mostly linear, but the Silhouette score peaked at k=2. I overrode the Silhouette recommendation because a 2-segment split offers no actionable business value, opting for k=4 where there was a local peak/recovery in the Silhouette plot.
- **Silhouette score, cluster sizes:** The Silhouette score was very low (close to zero / slightly negative), reflecting highly overlapping data. Cluster sizes: Cluster 0 (~93.6%), Cluster 1 (~0.04%), Cluster 2 (~3.5%), Cluster 3 (~3.2%).
- **Stability across seeds / subsamples:** The model is highly stable. Re-running KMeans with seeds 42, 123, and 999 consistently produced one massive cluster containing over 91-96% of the data, and three tiny outlier clusters. 

## 3. The segments (or anomalies)
- **Cluster 0:** No specific defining feature (Baseline). Persona: *The Average One-Off Buyer* (buys a random item once and never returns).
- **Cluster 1:** Defined by `moveis_colchao_e_estofado`. Persona: *The Furniture Buyer*.
- **Cluster 2:** Defined by `perfumaria` and `beleza_saude`. Persona: *The Beauty & Perfume Shopper*.
- **Cluster 3:** Defined by `bebes` and `brinquedos`. Persona: *The New Parent*.

## 4. Real or artifact?
- **Evidence your structure is real, and the weakness of that evidence:** The stability of the cluster sizes across different random seeds provides weak evidence of structure. However, the 2D PCA visualization explained only ~3.0% of the variance, showing a dense, inseparable cloud of points rather than distinct groups.
- **Any cluster that is likely an algorithm artifact?:** All of the clusters are essentially mathematical artifacts of KMeans trying to impose spherical shapes on highly sparse data. Instead of true behavioral segments, the algorithm merely dumped 94% of users into an "average" bucket and isolated three specific product niches into tiny outlier clusters.

## 5. Business action
- **Cluster 0 (Average Buyer):** Do not waste budget on targeted retention; use broad, automated seasonal discount emails.
- **Cluster 1 (Furniture):** Trigger a targeted cross-sell automated email for bed sheets (`cama_mesa_banho`) 2 weeks after delivery.
- **Cluster 2 (Perfume):** Trigger a "Replenishment Campaign" with a small discount on the same perfume brand 3 months post-purchase.
- **Cluster 3 (New Parent):** Implement an age-progressive marketing funnel (e.g., promote toddler toys 6-12 months after the initial baby product purchase).
(Structure Card)
```

## 5. Reflection
What surprised me most was the stark disconnect between algorithmic stability and business utility; a model can consistently converge on the exact same clusters across different random seeds, yet remain entirely useless if the underlying data is too sparse. While these extreme niche segments would likely reappear in new data due to the baseline sparsity of single-purchase e-commerce behavior, they would not hold up as a sustainable, long-term marketing strategy.

Applying these lessons to my upcoming work—whether clustering decision-making styles (System 1 vs. System 2) in my Sunk Cost Fallacy research or segmenting participant profiles within the Accelerator CRM module—I now completely understand the course's premise that the algorithm is only the easy 30%. The real data science work lies in engineering robust features beforehand, and rigorously interrogating the outputs to ensure they translate into actionable organizational strategies rather than just mathematical artifacts.
