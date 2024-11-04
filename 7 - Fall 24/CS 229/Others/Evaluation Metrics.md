
# Why Do We Care?
* Quantitiatively define a real world objective that we wouldn't be able to otherwise define.
	* Training objective (loss function) is typically a proxy for this real objective.
		* Ideally this objective matches the real world objective as closely as possible
	* E.g. can't quantify world health, so use GDP as a proxy

# Binary Classification Setting
![[Pasted image 20241030175058.png]]

# Score-based Models
![[Pasted image 20241030175113.png]]

## Classifiers
* Models need to set a threshold for the score and classify based on that.
![[Pasted image 20241030175203.png]]

# Point Metrics
## Confusion Matrix
![[Pasted image 20241030175223.png]]
* The top left (9) are the **true positives**
* The bottom right (8) are the **true negatives**
* The top right (2) are the **false positives** ⟶ Type I error
* The bottom left (1) are the **false negatives** ⟶ Type II error

## Accuracy
![[Pasted image 20241030175401.png]]

## Precision
![[Pasted image 20241030175410.png]]
![[Pasted image 20241030175624.png]]

## Positive Recall
* Also known as sensitivity.
![[Pasted image 20241030175436.png]]

## Negative Recall
* Also known as specificity.
![[Pasted image 20241030175451.png]]

## F-score
* F-score is the harmonic mean of precision and recall.
	* Penalizes both low recall and low precision.
	* F1 ensures a good balance between precision and recall.
![[Pasted image 20241030175514.png]]![[Pasted image 20241030175519.png]]

## Varying the Threshold
* Changing the threshold can result in a new confusion matrix, and new values for some of the metrics
* Many threshold values are redundant (between two consecutively ranked examples)
* Number of effective thresholds = # examples + 1
![[Pasted image 20241030175734.png]]

# Summary Metrics

## ROC Curve
* Each point on curve is defined by a threshold, corresponding to our point metrics.
	* Looks at a variety of confusion matrices by varying the threshold in order to generate the plot.
	* This plots (1 - specificity) against (recall) for all thresholds tested (in the table above)
* We want the area under the cur e to be as large as possible.
![[Pasted image 20241030175807.png]]

### AUROC
![[Pasted image 20241030175847.png]]
![[Pasted image 20241030181122.png|300]]


## PR Curve
* This curve plots precision against recall.
![[Pasted image 20241030175948.png]]

### AUPRC
* What is the expected precision when randomly picking threshold?
	* Recall precision is: out of model predicted positives, how many were actually positive?
	* Precision **focuses on the positive and negative class**. The AUROC focuses on the **negative class**.
![[Pasted image 20241030181138.png|300]]

## Log-loss
* These two models achieve the same values for most of the metrics so far. Which is better?
	* Same AUROC, AUPRC, point metrics etc. (same discrimination)
![[Pasted image 20241030180036.png|300]]

* Log-loss (cross entropy) rewards confident correct predictions and heavily penalizes confident incorrect predictions.
$$
-(y\log p + (1-y)\log(1-p))
$$
* So log-loss captures more than just discrimination
* It also captures calibration, i.e. how well the model’s predictions actually correspond to confidences
* Log-loss encourages calibration (proper scoring rule)

# Calibration Metrics

## Reliability Diagrams
![[Pasted image 20241030180604.png]]

## Techniques
![[Pasted image 20241030180617.png]]

# Class Imbalance
* Occurs when there is a significant imbalance between our classes.
	* Symptom: prevalence < 5%
		* When one class makes up <5% and the other class makes up >95% of dataset, etc
* **Problem:**
	* Metrics lose meaning
	* Inhibits learning
		* E.g. logistic regression can be overwhelmed by majority class

## Metrics
* Accuracy: when imbalanced, we can get high accuracy just by predicting majority class
	* This should be the low-bar!
* Log-loss: majority class can dominate
* AUROC: Can attain high AUROC by scoring negatives low
	* Artificially increased by true negatives
	* 10% prevalence. top-10% are all negatives, next are all the positives, followed by the rest of the negatives. AUROC = 0.9.
* AUPRC: Somewhat more robust, but other challenges
	* How do you interpolate?
* For class imbalance in general: Accuracy << AUROC << AUPRC

# Multi-class
* Confusion matrix will be $N \times N$ (still want heavy diagonals, light off-diagonals)
* Most metrics (except accuracy) generally analyzed as several 1-vs-many comparisons
* Class imbalance is common (both in absolute, and relative sense)
* Cost sensitive learning techniques (also helps in binary imbalance)
	* Assign weighted value for each block in the confusion matrix, and incorporate those into the loss function
