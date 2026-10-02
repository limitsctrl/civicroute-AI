\# CivicRoute AI - Request Category Prototype



Development for Artificial Intelligence - Course Assessment

Author: Thomas Hart Lindland



\## What this is

A small TensorFlow/Keras prototype that suggest a request category (pothole, water\_leak, broken\_streetlight, illegal\_dumping), so a council staff can route public service requests faster.

Decision support only, a human makes the final decision.



\## Repository structure



```

civicroute-AI/

|-- README.md
|
|
|
|-- .gitignore
\--Project
      |--civicroute_assessment.ipynb
      \--data
           |--civic_requests_prepared.csv
      |
      \-- notes
           |-- civic_assess_notes.ipynb
      |
      \-- outputs
           |-- confusion_matrix.png
           |-- training_curves.png
```



## How to reproduce

1. Python 3.12:
"pip install tensorflow, pandas, numpy, scikit-learn, matplotlib, jupyter"
2. Open "Project/civicroute\_assessment.ipynb"
3. Run \*\*Kernel -> Restart Kernel and Run All Cells\*\*
4. Results are reproducible: "RANDOM\_SEED = 2026" fixes the data split and model initialization





## Workflow summary

-| Step | What happens | Key decision |



- | Inspect | shape, types, category counts, missing values | balanced classes -> baseline 25% | 

- | Select | 9 input fields, target " category\_label" | excluded "request\_id", "night\_report\_flag", "neighbourhood" | 

- | Missing values | 23 of 7800 values (0.29%) | median for numeric fields, mode for "channel" | 

- | Encode | hour -> sin/cos (NumPy), urgency -> 0/1/2, channel -> one hot | keep real order, avoid invented order | 

- | Split | 70 / 15 / 15, stratified, fixed seed | scaling uses training data only | 

- | Model | Dense(32) -> Dense(16) -> Dense(4, SoftMax), 1044 parameters | small model for 420 training rows | 

- | Train | Adam, sparse categorical cross-entropy, early stopping | best epoch 35 | 



\## Key results

* Test accuracy 0.800\*\* vs 25% baseline (4 balanced classes)
* Strength:\*\* "illegal\_dumping" recognized reliably (21 of 23)
* Weakness:\*\* "pothole" and "broken\_streetlight" often confused (9 of 18 errors)
* Risky error:\*\* 4 of 22 water leaks routed to the wrong team



\## Limitations

* Small synthetic dataset; a single 90-row test set, so results are spproximate
* Fill values for missing data calculated before the split (mild, negligible leakage)
* Fairness across neighbourhoods not verified
* Decision support only: should not route requests automatically



