# Sparkify - Udacity capston project.

Blog is available at ==> https://medium.com/@kapur_naveen/sparkify-udacity-capstone-project-1e8959772937

Git repository link ==> https://github.com/nakapoor/Sparkify-Churn-Prediction

## Project Overview : 
Sparkify is a music app. The data provided here for the analysiss contains users information while intracting with the app. A user can contain many entries based on his actions. Along with users app activity data, data also shows a section of churned users through the cancellation of the account.

## Problem Statement : 
The purpose of the project is to understand the users activity data and to identify the characteristics of churned users from the behavioral data of these users, and take proper measures to retain the potential users who are likely to be churn based on the available data.

These are the below steps taken to achive the purpose.

## a) Data preprocessing [ For basic cleanup ]

Data contains few points where userid is missing these may be guest users. I am dropping any such data where userid is missing.

## b) EDA [Understanding of the Data]

Data set does not contains the target attribute so we need to understand the data and define the target attribute.
Defining churn : I have tried to identify the action "Cancellation Confirmation" which leads to the cancellation of the the user subscription and marked users with page entry as "Cancellation Confirmation" as churned users.

### There are 52 users performed the Cancilation confirmation request.
df.filter(df.page=="Cancellation Confirmation").select("userId").dropDuplicates().show(10)
+------+
|userId|
+------+
|   125|
|    51|
|    54|
|100014|
|   101|
|    29|
|100021|
|    87|
|    73|
|     3|
+------+

### User activity for user ID = 125.
df.select(["userId", "page", "time", "level", "song", "sessionId"]).where(df.userId == "125").sort("time").show(50)

+------+--------------------+-------------------+-----+--------------------+---------+
|userId|                page|               time|level|                song|sessionId|
+------+--------------------+-------------------+-----+--------------------+---------+
|   125|            NextSong|2018-10-12 04:05:44| free|    paranoid android|      174|
|   125|            NextSong|2018-10-12 04:11:21| free|Hypnotize(Album V...|      174|
|   125|            NextSong|2018-10-12 04:15:11| free|       I'm On My Way|      174|
|   125|            NextSong|2018-10-12 04:18:34| free|Leader Of Men (Al...|      174|
|   125|            NextSong|2018-10-12 04:22:04| free|       Love You Down|      174|
|   125|            NextSong|2018-10-12 04:28:35| free|Don't Leave Me Be...|      174|
|   125|            NextSong|2018-10-12 04:32:08| free|     They're Red Hot|      174|
|   125|            NextSong|2018-10-12 04:35:06| free|                Kota|      174|
|   125|         Roll Advert|2018-10-12 04:35:17| free|                null|      174|
|   125|              Cancel|2018-10-12 04:35:18| free|                null|      174|
|   125|Cancellation Conf...|2018-10-12 04:35:18| free|                null|      174|
+------+--------------------+-------------------+-----+--------------------+---------+

### Analysing the gender Distribution between churn user and normal user
df_withchurn.dropDuplicates(["userId", "gender"]).groupby(["churn", "gender"]).count().sort("churn").show()

+-----+------+-----+
|churn|gender|count|
+-----+------+-----+
|false|     M|   89|
|false|     F|   84|
| true|     F|   20|
| true|     M|   32|
+-----+------+-----+

#### rate of churn is hight for Male members.

## c) Feature Engineering [ Identifying and creating new features for model building ]

Worked on creating new features as mentioned below : 

### Adding Feature 1: gender as binary.
### Feature 2: Calculating number of days since registration [ life of a users with app]
### Feature 3: Total number of sessions/user
### Feature 4: Calculating avg, min & max time per session [ Time spent by user in app per session]
### Feature 5: number of songs per session.
### Feature 6: whether the account is a premium account or a free account
### Feature 7: Number of singers user heard.
### Target attribute : churn

### d) Modeling : 
For modeling purpose, I am using 3 algorithms

a) Logistic regression 

b) DecisionTreeClassifier

c) GBTClassifier


#### Results as followed :

a) Logistic regression  : 

Precision :  0.25

Recall :  0.17647058823529413

F1 Score :  0.20689655172413793


b) DecisionTreeClassifier

Precision :  0.5625

Recall :  0.5294117647058824

F1 Score :  0.5454545454545455

c) GBTClassifier

Precision :  0.5

Recall :  0.47058823529411764

F1 Score :  0.48484848484848486


<b> Conclusion </b> : DecisionTreeClassifier are performing best in all 3 algorithms. Looking at the results I can say that lot of work is required for inproving the predictions further. for improvement I am considering the below 2 approaches
a) Undersampling to optimize the F1 score.
b) I will look forward to build ensambling models for this problem.



