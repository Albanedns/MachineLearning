# MachineLearning

This final (and longer) project will give you the opportunity to have some fun while competing with other students to solve a real and difficult prediction task inspired by a biomedical research topic. Your job will be twofold:

build the best possible classification model on a training set to predict the (undisclosed) class labels on a given test set,
predict the actual classification performance your model will have on the test set.

# Design choices #
The "training" set is referred here in a broad sense, that is the fraction of the dataset on which the class labels are disclosed. It is up to you to decide what to do with this labeled set and, for instance, whether you split it (once or several times, possibly recursively) into actual training versus some validation fraction. More generally, this project is intended to be open as it is the case for a real task. Only the outcome matters! Many design choices are left open and you must specify them. Here is a non-exhaustive list of things you might have to consider.

Do you need to pre-process, to filter out, to normalize, etc., the available data? (look at the data,... LOOK AT THE DATA!)
How are you going to address the fact that some feature values could be numerical (either int or float) while others could be categorical or possibly boolean? Are there specific feature values only observed in the test but not in the training?
Should you consider all the available features? Should you define new or additional features from the existing ones? Should the newly defined features be crafted by hand, or generated automatically by other methods?
Which methodology are you going to use to learn a model, to fix its possible hyper-parameters and to predict its classification performance on the test set?
Which learning algorithm do you consider? You are free to choose the one you estimate more appropriate to the task. It needs not be (but it might be, of course) a learning algorithm that has been presented in the course. You can even consider several learning algorithms and/or produce and combine several models as long as your final prediction defines a unique label for each test example (ensemble methods? bagging?...).
What else?... The sky (and computing power) is the limit!

Competition performance metric
Since the dataset needs not be perfectly balanced in terms of class priors, the chosen test performance metric of a classifier is defined as the balanced classification rate (BCR). The BCR computes the classification rate for each class and reports the arithmetic average of those rates over all classes. In a binary classification context, BCR is simply the average between specificity and sensitivity. More generally, with C
 classes:

BCR=1C(∑i=1CTPini)
where:

ni
 denotes the number of examples from class i
 included in the test set,
TPi
 denotes the number of correctly classified examples from class i
 in the test set,
C
 denotes the number of classes.
Note that TPini
 is also denoted below as pi
 and simply refers to the proportion of correctly classified test examples from class i
.

Note also that a trivial classifier predicting all examples as belonging to the same class has a test BCR=1C
, no matter how the class priors are distributed (whenever the single predicted class is never actually observed in the test set, BCR = 0 assuming 00
 = 0.), while a perfect classifier has BCR=1
 (what is the expected BCR of a classifier predicting uniformly at random among the classes? Does it depend on the class priors?).

Since your task is not only to produce a model with the best possible BCR on the test set but also to predict how well your model is going to perform on this test set, we distinguish between the true test BCR
 of your model and your predicted test BCRˆ
 on this test set. The actual competition performance metric P
 (according to which you will be ranked and graded) is as follows:

P=BCR−Δ(BCR)⋅[1−exp(−Δ(BCR)σ )]
where:

Δ(BCR)=|BCR−BCRˆ|

σ=1C∑Ci=1pi(1−pi)ni−−−−−−−−−−√
, where pi
 is the proportion of correctly classified test examples from class i
 and ni
 is the number of test examples from class i
.
This competition performance metric is directly inspired from the International Performance Prediction Challenge WCCI 2006. The larger P
 the better. To maximize P
 you need to get the best possible test BCR
 and to make sure that your predicted BCRˆ
 is as close as possible to the actual test BCR
 of your model. Any deviation between both is penalized (by −Δ(BCR)
) while the influence of such penalty is limited in the region of uncertainty where Δ(BCR)
 is commensurate with σ
, the error bar on your true test BCR.

Here is a typical plot of the performance metric P
 as a function of the predicted BCRˆ
 for a true test BCR
 being equal to 70 %.

https://inginious.info.ucl.ac.be/course/LINFO2262/A5/compet_metric.png
The only thing you will need to provide for us to compute P
 is your predicted BCRˆ
 and the predicted class labels of the test examples. From this and knowing the undisclosed true class labels, we will compute the actual BCR
 of your model, its estimated error bar σ
 and the resulting P
. The highest P
 among all submissions will define the winner of the competition (and earn our congratulations!).

Note that you will not know your P
 score before the closing of the competition, not even your rank among the other competitors. This is the game!


The prediction task

You are a data analyst and machine learning expert in charge of helping medical researchers to improve the follow-up of patients suffering from breast cancer.

More precisely, you will have to analyze gene expression and clinical follow-up data in order to predict the subtypes of breast cancer tumors and, more specifically, the receptor status of these tumors.

https://inginious.info.ucl.ac.be/course/LINFO2262/A5/BiomedResearch.webp
DALL.E illustration generated on 2024-04-07 19.29.17.


The data includes 5,006 input features:

Gene expression values measured from DNA microarrays chips: 5,000 features (float) denoted G_0, ..., G_4999.
The time variable (int) recording the follow-up time (in days) of the patients after the initial diagnosis expressed.
The event variable (boolean) recording whether or not a distant metastasis occurred.
The node variable (boolean) recording whether or not lymph nodes are touched.
The sizeTum variable (float) describing the tumor size.
The grade variable (int) coding the breast cancer grade at the time of the diagnosis.
The age variable (int) recording the patient age (in years) at the time of diagnosis.

The output class to be predicted:

The label variable (categorical) includes 3 possible receptor status: ER+/HER2-, ER-/HER2-, HER2+.


The data, available on Moodle, is split into 3 files:

A5_2024_xtrain.gz which contains the 5,006 input features of 2,099 examples to train and validate your models.
A5_2024_ytrain.gz which contains the corresponding class labels of the 2,099 training (and validation) examples.
A5_2024_xtest.gz which contains the 5,006 input features of 370 test examples. It is your task to predict one class label for each test example.
