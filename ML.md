<link rel="icon" type="image/png" href="./imgs/favicon_db.png" />
<script src="https://cdnjs.cloudflare.com/ajax/libs/mermaid/8.0.0/mermaid.min.js"></script>
<script type="text/x-mathjax-config">
    MathJax.Hub.Config({
      tex2jax: {
        skipTags: ['script', 'noscript', 'style', 'textarea', 'pre'],
        inlineMath: [['$','$']]
      }
    });
  </script>
  <script src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script>

# Data science notes
## I/ Definitions

### 1) Vers Estimateur
#### Echantillon
Ensemble d'individus [*représentatifs*](https://fr.wikipedia.org/wiki/%C3%89chantillon_(statistiques)) d'une population.
#### Une statistique
Résultat d'une opération appliquée à un **échantillon**.
#### Variable aléatoire
Application définie sur l'ensemble des éventualités (=Ensemble des résultats possibles d'une expérience aléatoire)
#### Loi de probabilité
Loi décrivant le comportement d'une **variable aléatoire**.
#### Estimateur
**Statistique** permettant d'évaluer un paramètre inconnu relatif à une **loi de probabilité** (ex: espérance ou variance) 
Mesures de qualité :

|Mesure de $\hat\theta$|Valeur|
|--|--|
|Biais|$Biais(\hat \theta)=E[\hat \theta]-\theta$|
| Erreur quadratique moyenne | $MSE(\hat \theta)=E[(\hat \theta-\theta)^2]$ |
|Convergence|$\lim \limits_{n\rightarrow \infty} I\kern-.9ex P(\mid\hat \theta_n -\theta\mid>\epsilon)=0, \space \forall\epsilon>0$|
|Convergence forte|$I\kern-.9ex P(\lim \limits_{n\rightarrow \infty} \hat \theta_n =\theta)=1$|
|Efficacité|$Var(\hat{\theta})=E[\hat{\theta}^2]-E[\hat{\theta}]^2$|
|Robustesse|Sensibilité aux *outlayers*|

### 2) Vers test du $\chi^2$
[StatQuest channel](https://www.youtube.com/channel/UCtYLUTtgS3k1Fg4y5tAhLbw)
#### Loi normale
$densité(x)=\frac{1}{\sigma \sqrt{2\pi }}e^{-\frac{(x-\mu)^2}{2\sigma^2}}$
#### Théorème central limite
https://machinelearningmastery.com/a-gentle-introduction-to-the-central-limit-theorem-for-machine-learning/
#### Loi du $\chi^2$
#### Hypothèse nulle
Postule l'égalité entre des **statistiques**  de deux échantillons différents, supposé pris sur des populations équivalentes.
#### p-valeur
#### Test statistique
[Crash Course Channel](https://www.youtube.com/watch?v=QZ7kgmhdIwA)

Peut-on rejeter l'hypothèse nulle ?
Ils retournent une valeur-p (*p-value*) ou une *critical value* à comparer à des seuils conventionnels pour conclure.
- Le rejet d'un test peu être rechercher pour s'assurer que les pattern intéressants observés ne sont pas dus au hazard
#### Côte Z (z-score)
$z=\frac{x-\mu}{\sigma}$

#### t-test
Approximation du z-test si on ne connait qu'un échantillon de la population
#### Test du $\chi^2$
[CrashCourse channel](https://www.youtube.com/watch?v=7_cs1YlZoug)
### 3) Loss functions 

Given:
-  an estimator $\hat f$
-  a set of samples $X$
- a vector $y$, the true labels associated:
 $\forall x^{(i)} \in X, y^{(i)}=f(x^{(i)})+\epsilon^{(i)}$
##### a) 0-1 loss
$$
loss_{0-1}(\hat f, X, y) = \sum\limits_{i=1}^{|X|}I_{y^{(i)} \ne \hat f(x^{(i)})}
$$
- Has sense only for classification
- It's simply count of missclassifications. 
- $accuracy(\hat f, X,y)=1-\frac{1}{|X|}loss_{0-1}(\hat f, X, y)$
##### b) $L_1$ loss = **Least Absolute Deviations** (LAD)
$$
loss_{L_1}(\hat f, X, y) = =\sum\limits_{i=1}^{|X|}|y^{(i)} - \hat f(x^{(i)})|
$$
- More robust if $(X,y)$ contains outliers
##### c) $L_2$ loss = **Least Squared Errors** (LS)
$$
loss_{L_2}(\hat f, X, y) = \sum\limits_{i=1}^{|X|}(y^{(i)} - \hat f(x^{(i)}))^2
$$
- Prefered choice in general case

#### ⚠️⚠️⚠️
Do not use $loss_{L_2}$ nor $loss_{L_1}$ in classification if activation function output can be greater than $1$:
<img src="https://qph.fs.quoracdn.net/main-qimg-9b7f05954e9318800bb453f10385c9ca">
We don't want correctly classified samples with activation function output $>1$ to be involved in weights updates because it might slow the learning.

## II/ Bias-variance tradeoff
[Incredibly clear explanation by Scott Fortmann](http://scott.fortmann-roe.com/docs/BiasVariance.html)

Training set $x_1,...,x_n$ with $y_i$ associated with each sample.

### ML models as estimators
**Estimateur** *: Statistique permettant d'évaluer un **paramètre inconnu** relatif à une* **loi de probabilité**

- $E_X=E_{x^{(1)}} \times E_{x^{(2)}} ...\times E_{x^{(J)}}$ l'ensemble des vecteurs de variables
- $E_y$ l'ensemble des labels
- **Loi de probabilité**: Notre variable aléatoire est la collecte de données labellisées, notons la $D$, ensemble d'éléments de $E_X\times E_y$.
- We assume that there is a relation de $D_{E_X}\rightarrow D_{E_y},  x\mapsto y=f(x)+\epsilon$, avec $f:E_X\rightarrow E_y$.
$\epsilon$ is the noise, zero mean and $\sigma^2$ variance.
- Notre **paramètre inconnu** est $f$.
- Soit l'**estimateur** de $f$ sur la loi de probabilité $D$ noté $\hat f$. 
- Soit $(x, y) \in E_X\times E_y$ mais $(x, y)\notin D$, $E[(y-\hat{f}(x))^2]$ est décomposable en la somme de trois termes:


|the square of the _bias_ of the learning method|the _variance_ of the learning method|the irreducible error|
|--|--|--|
|$(E[\hat{f}(x)]-f(x))^2$|$E[\hat{f}(x)^2]-E[\hat{f}(x)]^2$|$\sigma^2$|
|the error caused by the simplifying assumptions built into the method|how much the learning method $\hat{f}(x)$ will move around its mean |Since all three terms are non-negative, this forms a lower bound on the expected error on unseen samples|

Le meilleur modèle a une complexité $c_0$ telle que: $$\frac{d(Bias^2)}{d(complexité)}(c_0)=-\frac{d(Variance)}{d(complexité)}(c_0)$$
```python
import scimple as scm
%matplotlib notebook
scm.Plot(title="Bias-variance tradeoff", borders=[0.3,2, -1,5])\
.add(x=scm.xgrid(0.3,2,0.01), 
     y=lambda i, x: 1/x[i]**2, marker="-", label="Bias²")\
.add(x=scm.xgrid(0.3,2,0.01), 
     y=lambda i, x: x[i]**2, marker="-", label="Variance")\
.add(x=scm.xgrid(0.3,2,0.01), 
     y=lambda i, x: 0.5, marker="-", label="NoiseError")\
.add(x=scm.xgrid(0.3,2,0.1), 
     y=lambda i, x: 0, marker="+", markersize=1)\
.add(x=scm.xgrid(0.3,2,0.01), 
     y=lambda i, x: 1/x[i]**2 + x[i]**2 + 0.5, 
     marker="-", label="Error")\
.add(x=scm.xgrid(0.3,2,0.01), 
     y=lambda i, x: scm.derivate(lambda z: 1/z**2 + z**2 + 0.5, x[i]), 
     marker="-", label="d(Error)")
```
## Ensembling
### Bagging
## Evaluation, Model selection
http://scott.fortmann-roe.com/docs/MeasuringError.html
### R²
### ROC receiver operating characteristic
- For a classifier
- Need its implementation to let you access some sort of score instead of flat class prediction (it's always doable if you have access to sources).
- You then **move a threshold** on the entire domain of the score and you can associate to each threshold a confusion matrix showing how well your classifier separate samples.

|  |  ||
|--|--|--|
|predicted $\setminus$  actual| P | N|
| P | TP |FP|
| N | FN |TN|

- Plot parametric courb 
$$\left\{  
\begin{array}{l}  
x = TPR(\theta) \\  
y = FPR(\theta)
\end{array}  
\right.$$


Actual positives well classified rate $TPR=\frac{TP}{TP+FN}$
Actual negatives miss classified rate $FPR=\frac{FP}{FP+TN}$

We suppose our classifier say $P$ if $score\geq\theta$
Moving $\theta$ in its interval $[a,b]$, here are special cases:
 - Common cases to all classifiers:
   - $\theta=a$: all samples are classified positive. Every actual negative are missclassified and every actual positives are well classified, courb is at $(1,1)$.
   - $\theta=b$: all samples are classified negative. No actual negative is missclassified and no actual positives is well classified, courb is at $(0,0)$.
 - $\theta\in ]a,b[$:
   - Ideal Classifier: ideal classifier gives score $a$ to all actual negatives and score $b$ to all actual positives, so if threshold is in between it will do perfect job with no actual negatives missclassified and all actual positives well classified, courb is at $(0,1)$. Note, ROC of ideal classifier has only three points, $(1,1),(0,1),(0,0)$.
   - Uniform random Classifier: uniform random classifier gives a uniformaly random score $\in [a, b]$ to each sample. So $TPR(\theta)=\frac{\theta}{b-a}$ and $FPR(\theta)=\frac{\theta}{b-a}$, resulting in an identity courb $TPR(FPR)=FPR$.

### AUC
Area under curve $\in [0,1]$, after normalization.
### Crossval
### Hyper params tuning
#### GriSearch
#### Bayesian approach
## Prép
### Kernel trick