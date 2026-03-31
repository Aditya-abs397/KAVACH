SESSION-01

CREATING BASELINE ANAMOLY DETECTING MODEL-

-Did data preprocessing.
-Applied Isolation forest model.
-Evaluated model on classification metrics.
               
# Metrics

         Precision   Recall  f1-score support

-Normal     0.90      0.86     0.88    3685 

-Attack     0.22      0.30     0.25    492

-accuracy                      0.79    4177   


# Why F1 score is low?

-Contamination was set to 0.12 which was too low, model misses attacks which is indicated 
 by low recall value which is 0.30.
-Did not tuned any hyperparameters as of now.
-Also model had low precision meaning it suffered from high False Positives.

--FALLBACKS--

-TEMPORAL BLINDNESS:
 The model treated each timestamp as an isolated event,failing to account for the time series nature of water tank levels and pump pressures.

-Global vs local-anomalies:
 The model flagged statistical outliers as attacks, but in ICS, many attacks are "stealthy" and stay within normal operating ranges while drifing towards a failing state.

--LEARNINGS--

In real world cyber-physical systems, the 0.25 F1 score signifies a significant rist due to its low precision. Hence increasing F1 scores is necessary.

