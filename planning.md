# TakeMeter Project Planning

## Community

For this project, I chose **r/soccer**, one of Reddit's largest sports communities.

The subreddit contains discussions about soccer leagues, international tournaments, players, tactics, and match results from around the world.
This community is a strong fit for a classification task because the dataset is highly varied. Within a single discussion thread, users may provide tactical analysis, make predictions, express emotional reactions, or post jokes and memes. These create meaningful categories that are recognizable to regular members of the community.

---

## Labels

### 1. Analysis

**Definition:** A comment that evaluates players, teams, tactics, or matches using reasoning, evidence, or football knowledge. Includes subjective judgments if they are supported by observation or argument.

**Example 1:**
"Saliba and Upamecano have not been seriously tested yet. They spend most of their game recovering loose balls and giving it back to their midfield to start attacks."

**Example 2:**
"Mourinho used a system where the full back would man mark Messi till certain regions of the field, after which other players would take over. Communication and proper handoff between players was critical for this to work."
---

### 2. Prediction

**Definition:**A comment that forecasts a future outcome or discusses a hypothetical future scenario. Includes all forecasts about future match outcomes, even if emotional or speculative.

**Example 1:**
"Seeing the Norwegian defense try to deal with France's attack will be a sight to see."

**Example 2:**
"I think Argentina will win because their midfield controls games better."
---

### 3. Reaction

**Definition:** A comment expressing pure emotion, excitement, awe, or personal feeling with no claim being made. Also covers bare assertions about a player, team, or performance made without supporting reasoning or evidence.

**Example 1:**
"I'm thankful to be witnessing this, and I'm gonna miss him the day he retires."

**Example 2:**
"We are all unbelievably lucky to be born at a time to watch this man play football."

**Example 3:**
"39 year old Messi is head and shoulders above everyone in this tournament."

**Example 4:**
"He could still easily play in Europe if he wanted to. Would just need a hard working team around him."

---

### 4. Humor

**Definition:**A comment whose primary purpose is humor, sarcasm, a joke, or a meme reference. If it asserts a football outcome, it is NOT Humor even if sarcastic.

**Example 1:**
"Jordi Alba literally made an entire career out of two things: if Messi has ball, make a run. If you have ball, pass to Messi. And he was a top 5 left back for a decade doing that."

**Example 2:**
"Before anyone feels bad for Algeria, the real victims are the Portugal players watching a certain teammate trying to score from the edge of the box while they are completely free."

## Hard Edge Cases
Opinion vs Analysis:

If the comment makes a claim AND provides reasoning, tactical observation, or evidence to support it → Analysis. If it states the claim with no support → Reaction.

Example: "Messi is still the best in the world" → Reaction. "Messi is still the best because Argentina build every attack around finding him in space, and no defender has figured out how to cut that off" → Analysis.

Decision rule: Remove the opinion. Does anything substantive remain? If yes → Analysis. If nothing remains → Reaction.

Reaction vs Prediction:

If a feeling is expressed with no underlying forecast → Reaction. If a future outcome is being predicted, even with emotional tone → Prediction.

Example: "I COULD CRY" → Reaction. "He's going to win the golden boot" → Prediction.

Decision rule: Is there a future outcome being stated? If yes → Prediction. If it's purely expressive → Reaction.

Humor vs Prediction:

If a comment asserts a football outcome even sarcastically → Prediction. Humor is only used when the main intent is a joke with no real claim being made.

Analysis vs Reaction:

If a comment evaluates tactics, patterns, or performance with specific observation → Analysis. If it evaluates without any supporting observation → Reaction.

**Example 1:**

"mbappe might take the record this WC with how he's been scoring. Haaland probably won't be able to catch up with how mbappe's scoring"

Possible labels: Prediction, Analysis
Decision: Prediction
Reason: The comment forecasts a future outcome (Mbappe taking the record). The reference to current scoring pace feels like evidence for the forecast, not independent tactical analysis. Decision rule: if the main point is a forecast and the reasoning just supports it, label the forecast.


**Example 2:**

"It is ridiculous. But I do think tbf Senegal are a much better team than anyone Messi has played yet."

Possible labels: Reaction, Analysis
Decision: Analysis
Reason: The second sentence makes a comparative evaluative claim about opponent quality — this is reasoned judgment about team strength, not just a bare assertion. Decision rule: if a claim is qualified with a comparative observation that could be fact-checked, it leans Analysis over Reaction.


**Example 3:**

"Viking demands a blood sacrifice from France and Argentina"

Possible labels: Humor, Reaction
Decision: Humor
Reason: The primary intent is a joke — framing Haaland's performance as a Viking ritual. There's no claim and no genuine emotional response being expressed. Decision rule: if the framing is entirely constructed for comedic effect and the literal reading makes no sense, it's Humor regardless of tone.
---

## Data Collection Plan

I will collect comments from discussion threads on r/soccer community , focusing on match threads, post-match threads, tournament discussions, and news posts.

My goal is to collect at least **200 labeled comments**.

Target distribution:

* Analysis: 40 comments
* Prediction: 40 comments
* Reaction: 80 comments
* Humor: 40 comments

Reaction is higher because it covers both pure emotional responses and bare unsupported assertions (previously a separate Opinion label that was merged for model compatibility). At 40% it remains well under the 70% imbalance threshold.

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
Annotation Priority Rules:

1. If the main purpose is a joke/meme → Humor
2. If the main purpose is predicting a future outcome → Prediction
3. If the comment explains why something is true → Analysis
4. If the comment only expresses emotion or makes a bare unsupported assertion → Reaction

I will use ChatGPT to pre-label examples before manual review. For each Reddit post, I will provide the title and text to ChatGPT along with my label definitions and ask for a predicted label. I will then review every prediction myself and make the final annotation decision.

## AI Tool Plan

### Label stress-testing

I will use ChatGPT to generate 5–10 boundary-case posts between labels. If I cannot classify them cleanly, I will revise my label definitions before annotation.

### Annotation assistance

I may use ChatGPT to suggest initial labels for posts. I will review every suggestion and assign the final label myself.

### Failure analysis

I will use ChatGPT to help identify patterns in misclassified examples after evaluation. I will verify any suggested patterns manually using the confusion matrix and original examples.

### AI usage

ChatGPT will be used for planning, label testing, optional pre-labeling, and error analysis, but not for final dataset labels.

**Model: distilbert-base-uncased**
Hyperparameter changes from defaults:
Parameter  Default Final 
num_train_epochs 3 10
learning_rate 2e-5 5e-5


Why:

With only 112 training examples, 3 epochs was insufficient — the model collapsed to predicting the majority class on every input. Increasing to 10 epochs gave the model more passes through the data. load_best_model_at_end=True ensures the best validation checkpoint is used regardless of final epoch.
Learning rate was increased to 5e-5 because small datasets benefit from a faster update rate to escape the majority-class trap. The standard 2e-5 was too conservative given the dataset size.

**Wrong Prediction 1 — Reaction misclassified as Analysis (confidence: 0.93)**

"Don't care what anyone says. Messi is still the best in the world. He could still easily play in Europe if he wanted to. Would just need a hard working team around him like he has with Argentina."

True: reaction | Predicted: analysis
The model was fooled by evaluative vocabulary — "best in the world", "play in Europe", "hard working team" are all football-judgment phrases it associates with Analysis. But no reasoning supports the claim; it's a pure assertion. This reveals the model learned surface vocabulary patterns rather than the presence or absence of supporting argument. High confidence (0.93) suggests it was very wrong in a very certain way.

**Wrong Prediction 2 — Humor misclassified as Reaction (confidence: 0.95)**

"My favorite part of when he joined Miami is seeing him go into grocery stores without people swarming him. Nobody knew who he was lol."

True: humor | Predicted: reaction
The joke here is implied — Messi being unknown at a grocery store is absurd given his fame. But the comment is written in a conversational, personal tone ("My favorite part") that reads like a genuine emotional response. The model never learned to detect implied absurdity as a humor signal, defaulting to Reaction for anything that sounds personal and expressive.

**Wrong Prediction 3 — Humor misclassified as Prediction (confidence: 0.84)**

"I've seen enough, Morocco is the 2025 AFCON champion"

True: humor | Predicted: prediction
This is a documented edge case from your planning.md — sarcastic outcome claims. The comment is clearly a joke (making a definitive tournament prediction based on one game) but structurally it looks exactly like a Prediction. The model correctly identified the future-outcome structure but missed the sarcastic intent entirely. This is the hardest Humor subtype to classify even for humans.