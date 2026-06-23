# TakeMeter Project Planning

## Community

For this project, I chose **r/soccer**, one of Reddit's largest sports communities.

The subreddit contains discussions about soccer leagues, international tournaments, players, tactics, and match results from around the world.
This community is a strong fit for a classification task because the dataset is highly varied. Within a single discussion thread, users may provide tactical analysis, make predictions, express emotional reactions, or post jokes and memes. These create meaningful categories that are recognizable to regular members of the community.

---

## Labels

### 1. Analysis

**Definition:** A comment that evaluates players, teams, tactics, or matches using reasoning, evidence, or football knowledge.Includes subjective judgments about player/team quality if they are reasoned.

**Example 1:**
"Saliba and Upamecano have not been seriously tested yet. They spend most of their game recovering loose balls and giving it back to their midfield to start attacks."

**Example 2:**
"Except they aren't world class in every position now. Certainly not world class in midfield and fullbacks and goalkeeper."

---

### 2. Prediction

**Definition:** A comment that forecasts a future outcome or discusses a hypothetical future scenario.Includes all forecasts about future match outcomes, even if emotional or speculative.

**Example 1:**
"Seeing the Norwegian defense try to deal with France's attack will be a sight to see."

**Example 2:**
"Please no, we're gonna be eliminated by Japan."

---

### 3. Reaction

**Definition:** A comment that expresses an opinion, emotion, excitement, or personal response without substantial reasoning.Does not include structured evaluation of performance or tactics.

**Example 1:**
"The only solution is to just smash France's defense and score even more goals."

**Example 2:**
"Haaland who once scored 9 goals vs Honduras, we are calling for you."

---

### 4. Humor

**Definition:** A comment whose primary purpose is humor, sarcasm, a joke, or a meme reference.If it asserts a football outcome, it is NOT Humor even if it is sarcastic.

**Example 1:**
"Bold strategy, cotton."

**Example 2:**
"Ah you mean PSG vs Bayern but on an international level."

---

## Hard Edge Cases
The hardest edge cases are:

Analysis vs Reaction
If the comment evaluates skill, tactics, or performance, it is Analysis. If it only expresses emotion or hype without structured reasoning, it is Reaction.

Prediction vs Reaction
If the comment states or implies a future match outcome, it is Prediction. If it expresses desire or excitement without asserting outcome, it is Reaction.

Humor vs Prediction
If a comment asserts a football outcome (even sarcastically), it is Prediction. Humor is only used when the main intent is joking or meme behavior rather than making a claim.
---

## Data Collection Plan

I will collect comments from discussion threads on r/soccer community , focusing on match threads, post-match threads, tournament discussions, and news posts.

My goal is to collect at least **200 labeled comments**.

Target distribution:

* Analysis: 50 comments
* Prediction: 50 comments
* Reaction: 50 comments
* Humor: 50 comments

If one label is underrepresented after collecting 200 comments, I will intentionally gather examples from threads for the required types. For example, if Humor comments are rare, I will collect more comments from live match threads where jokes occur frequently.

The final dataset will be split into training, validation, and test sets.

---

## Evaluation Metrics

I will evaluate the model using multiple metrics:

### Accuracy

Accuracy measures the percentage of comments correctly classified overall. It provides a simple measure of model performance but does not reveal how well individual labels are handled.

### Precision

Precision measures how often predictions for a label are correct. This is important because I want the model to avoid incorrectly labeling comments as Analysis, Prediction, Reaction, or Humor.

### Recall

Recall measures how many examples of a label the model successfully identifies. This is important because missing large numbers of comments from a particular category would make the classifier less useful.
### Confusion Matrix

A confusion matrix will help identify systematic mistakes. For example, it may reveal that the model frequently confuses Analysis with Prediction or Reaction with Humor.

---
## Definition of Success

The classifier will be considered successful if it achieves:

* At least 75% overall accuracy on the test set.
* No label with precision or recall below 0.60.
* Better performance than the zero-shot Groq baseline by at least 5 percentage points in accuracy.

### Label Stress Test

#### Post 1

"France are going to destroy Norway. Their midfield is just on another level."

Possible labels:

* Analysis
* Prediction

Decision:
Prediction

Reason:
The primary purpose is forecasting a future result. The midfield comment serves as justification.

---

#### Post 2

"Saliba is the best center back in the world right now."

Possible labels:

* Analysis
* Reaction

Decision:
Reaction

Reason:
No supporting reasoning is provided. It is an opinion rather than an argument.

---

#### Post 3

"If Haaland gets decent service, France could genuinely be in trouble."

Possible labels:

* Analysis
* Prediction

Decision:
Prediction

Reason:
The main purpose is predicting a future outcome.

---

#### Post 4

"I'd pay good money to watch Haaland bully that French defense."

Possible labels:

* Prediction
* Reaction

Decision:
Reaction

Reason:
The comment expresses excitement and desire rather than a forecast.

---

#### Post 5

"Norway's defensive strategy appears to be hoping Haaland scores six goals."

Possible labels:

* Analysis
* Humor

Decision:
Humor

Reason:
The statement is primarily sarcastic.

---

#### Post 6

"Watch Norway somehow win the whole thing after everyone wrote them off."

Possible labels:

* Prediction
* Humor

Decision:
Prediction

Reason:
Despite the playful tone, it is still forecasting an outcome.

---

#### Post 7

"France have looked vulnerable on set pieces all tournament."

Possible labels:

* Analysis
* Reaction

Decision:
Analysis

Reason:
It evaluates team performance rather than expressing a feeling.

---

#### Post 8

"It would be hilarious if Sweden knocked Norway out next round."

Possible labels:

* Prediction
* Humor

Decision:
Humor

Reason:
The focus is the joke rather than the forecast.

---

#### Post 9

"Norway are absolutely cooked."

Possible labels:

* Reaction
* Prediction

Decision:
Reaction

Reason:
It expresses an opinion without explicitly predicting a future result.

---

#### Post 10

"Odegaard has been creating chances every game, so I think Norway have a real shot."

Possible labels:

* Analysis
* Prediction

Decision:
Prediction

Reason:
The conclusion is a forecast, supported by evidence.
## Annotation Assistance

I will use ChatGPT to pre-label examples before manual review. For each Reddit post, I will provide the title and text to ChatGPT along with my label definitions and ask for a predicted label. I will then review every prediction myself and make the final annotation decision.

## AI Tool Plan

### Label stress-testing

I will use ChatGPT to generate 5–10 boundary-case posts between labels. If I cannot classify them cleanly, I will revise my label definitions before annotation.

### Annotation assistance

I may use ChatGPT to suggest initial labels for posts. I will review every suggestion and assign the final label myself.

### Failure analysis

I will use ChatGPT to help identify patterns in misclassified examples after evaluation. I will verify any suggested patterns manually using the confusion matrix and the original examples.

### AI usage

ChatGPT will be used for planning, label testing, optional pre-labeling, and error analysis, but not for final dataset labels.


