# TakeMeter — planning.md

> Complete this document before you collect a single labeled example.
> It must substantively address the six questions below — real, specific answers, not placeholders.
> Your planning.md will be reviewed as part of your submission.
> Update it before starting any stretch features, and add your hard annotation decisions to
> "Hard Edge Cases" as you hit them during Milestone 3.

---

## 1. Community

**What community did you choose and why?**
<!-- Name the specific community (e.g. a subreddit, Discord, forum). One sentence. -->
I chose the community ``r/AmITheAsshole``
due to the consistent structure of post through auto moderation and active mods. I also find the subreddit interesting to read through.
**Why is this community a good fit for a classification task — what makes the discourse varied enough to be interesting?**
<!-- Why can you collect 200+ public examples from it? What is your unit of classification — posts or comments? -->
The community has a voting system where people can upvote a comment with the rating they agree with or create their own reply with a rating. The ratings are a built in label for classification which I can use to make additional labels from that baseline.
Ratings:
+ NTA/YWNBTA = You are/would be not the asshole (but the other party is)
+ YTA/YWBTA = You are/would be the asshole (and the other party is not)
+ ESH = Everyone sucks here (you and the other party are both the asshole)
+ NAH = No assholes here (no one’s an asshole)
+ INFO = More information needed


---

## 2. Labels

> What are your 2–4 labels? Each must be mutually exclusive, exhaustive enough to cover ≥90% of
> examples without an "other" bucket, and grounded in how this community actually talks.
>


### Label 1: `<judgement>`


**Define this label in a complete sentence:**
Votes on judgement without reasoning 

<!-- State the decision boundary so clearly that two people would agree on most examples. -->




**2 example posts:**
- Example A:
  + NTA. "Girl mom no longer talks to me.". I love it when problems solve themselves
- Example B:
  + Soooo NTA. That's really creepy. And bad parenting. 


---

### Label 2: `<reasoning>`

**Define this label in a complete sentence:**
reasons through the situation using specifics from the post, works toward a conclusion



**2 example posts:**
- Example A:
  - It sounds like the girl's reaction was real, so I'd be more inclined to say this is mom capitalizing on something that naturally happened." 
- Example B:
  - The girl already took responsibility and apologized and you accepted it. That's the end. This was clearly just her mom trying to create content, and neither the girl nor you were actually interested in being in it


---

### Label 3: `<speculation>`

**Define this label in a complete sentence:**
The comment's main point is about someone's unstated motive, intent, or character, whether thats the OP or the other person, beyond what the post actually states


**2 example posts:**
- Example A:
  - My guess is she planned to post it on social media and show what a "good mom she is" for forcing her kid to apologize formally and the cake was to make it more niche.
- Example B:
  - I thought she was intentionally making her daughter feel like crap and wasn’t going to let it die until she had her hallmark moment.


---

## 3. Hard Edge Cases

**What type of post will be genuinely ambiguous between two labels?**
<!-- Describe at least one post type that sits on the boundary. Name which two labels it could be.
     If you can't name a hard case, you haven't pushed on your boundaries enough. -->
The hardest one is reasoning vs speculation. Both are long comments that build out an argument so they look the same at first glance. The difference is what they end up concluding about. Reasoning stays on the actions and situation the post actually describes. Speculation jumps to someone's motives or character, the OP's or the other person's, that the post never states. There is also a smaller overlap between judgement and reasoning when a short comment tacks on one reason, like "NTA she already apologized". A verdict word by itself does not make a comment judgement, what matters is what the comment is mostly doing.

**How will you handle it when you encounter it during annotation?**
<!-- Write the decision rule you'll apply, in a sentence or two, so you label consistently. -->
If the comment's main point is what happened and whether it was justified, I label it reasoning. If its main point is why someone secretly did something, their hidden motive or character, whether thats the OP or the other person, I label it speculation, even when it starts from a real fact from the post. For the short ones, if it points to a specific fact from the post to back up the verdict it counts as reasoning even when its short. If it only states a verdict or agrees with one, it is judgement.

**Difficult cases encountered while annotating (fill in during Milestone 3):**
<!-- Log at least 3 real posts that gave you pause: what it was, which labels it could be,
     and what you decided. These also go in your README. -->
I will fill these in as I hit them during annotation in Milestone 3.
1.
2.
3.

---

## 4. Data Collection Plan

**Where will you collect examples?**
<!-- Specific source(s). Public only — no private channels or content behind authentication. -->
I am collecting public top level comments from r/AmITheAsshole. My plan is to scrape them first using Reddit's public json, and if the results come out messy or hard to clean I will switch to copying comments in by hand. Either way I am staying close to the data and not letting collection turn into a big coding project. My unit is comments, not posts.

**How many per label?**
<!-- Aim for ≥20% per label and no label >70%. State your per-label target. -->
I am aiming for about 70 per label, around 210 total, so every label is over 20% and none goes past 70%. Reasoning is going to be the thin one because most AITA comments are quick verdicts or guesses about the other person. To get enough reasoning I will pull from contested and controversial sorted threads where people actually have to argue their case.

**What will you do if a label is underrepresented after 200 examples?**
<!-- Your concrete plan for rebalancing — e.g. targeted extra collection for the thin label. -->
If reasoning is still short after 200, I will go back to the mixed verdict and controversial threads and collect more of it on purpose. I also want to grab some short reasoning comments and some long judgement comments on purpose, so the model doesnt just learn short vs long instead of what the comment is doing.

**File format:**
<!-- One CSV with columns text, label, notes. Do NOT pre-split — the notebook splits 70/15/15. -->
One CSV with the columns text, label, and notes. I am not pre splitting it, the notebook does the 70/15/15 split.


---

## 5. Evaluation Metrics

**Which metrics will you use to evaluate your model, and why are those the right ones for this specific task?**
<!-- Accuracy alone is not enough — explain what else you need and why (per-class precision/recall/
     F1, macro-F1, confusion matrix). Tie each metric to something specific you want to catch,
     e.g. a minority class the model might ignore. -->
I am using per class precision, recall, and F1, plus macro F1 and a confusion matrix. Accuracy on its own would hide my real problem, because reasoning is the small class and a model could basically ignore it and still look fine on accuracy. Macro F1 weights every class the same so it catches that. I care most about recall on reasoning since that is the class most likely to get dropped. The confusion matrix is there so I can watch the reasoning and speculation cell, which is my hardest boundary, and also judgement vs reasoning. On top of that I want to check my errors by comment length, to see if the model is really just learning short vs long instead of the actual content.


---

## 6. Definition of Success

**What performance would make this classifier genuinely useful?**
<!-- A specific, measurable threshold you could objectively check at the end — not "it works well."
     e.g. "beats the baseline on macro-F1 and every class has F1 ≥ X." -->
It is genuinely useful if the fine tuned model beats the Groq baseline on macro F1 and every class lands at an F1 of at least 0.60, with reasoning recall at least 0.50 since that is the class I expect to struggle.

**What would you accept as "good enough" for deployment in a real community tool?**
<!-- A higher bar than minimum-useful. State the numbers. -->
For something I would actually put in a community tool I want macro F1 of at least 0.75, every class at F1 of 0.70 or higher, and the errors not coming mostly from the length issue, so roughly even error rates on short and long comments.


---

## AI Tool Plan

> No code to generate here — these are the three places AI tools actually help on this project.
> Make an explicit decision about each, even if the decision is "skip."

**Label stress-testing:**
<!-- Will you give the AI your label definitions + edge cases and ask it to generate 5–10 posts on
     the boundary, then tighten your definitions if you can't classify them cleanly? -->
Yes. I am giving the AI my three definitions and edge case rules and asking it to write 5 to 10 comments that sit on the boundary, mostly reasoning vs speculation. If I cant sort them cleanly, I tighten the definitions before I annotate 200. I did this and it made me change two of my definitions, the full run is in the Stress Test section at the bottom.

**Annotation assistance:**
<!-- Will you use an LLM to pre-label a batch before reviewing every one yourself? If yes: which tool,
     and how will you track which rows were pre-labeled (for disclosure in your AI usage section)? -->
Yes. I am having an LLM pre label the comments first instead of labeling everything from scratch, then I go through and review and fix every row myself. I will mark the pre labeled rows in the notes column so I know which ones came from the LLM, and I will disclose this in the README.

**Failure analysis:**
<!-- Will you paste your wrong predictions into an AI tool to surface patterns? What will you look for,
     and how will you verify the patterns yourself before writing them up? -->
Yes. I will paste my wrong predictions into the AI and ask it for patterns, like the length issue or one label pair that keeps getting confused. Then I will re read the examples myself to confirm the pattern is real before I write it up.


---

## Stress Test

Before annotating I ran a label stress test. I gave an LLM my three definitions and edge case rules and asked it to write 9 boundary comments without labeling them, then I labeled them myself to see if my rules held up.

| # | Comment | My label |
| --- | --- | --- |
| 1 | NTA, and the fact that you paid for the whole dinner just makes it more obvious. Anyone who shows up two hours late to a meal someone else is covering doesn't get to complain about the food being cold. | reasoning |
| 2 | YTA here. You read her diary, then confronted her about what you found. That's two separate violations stacked on top of each other, and the second one only happened because of the first. | reasoning |
| 3 | NTA. You asked him to move his car three times before you blocked him in. Three requests, three refusals, then a consequence. That's not an overreaction, that's cause and effect. | reasoning |
| 4 | NTA. She "forgot" your birthday the same year she threw herself a huge party and didn't invite you. People don't forget the things that matter to them, so the message is pretty clear about where you rank. | speculation |
| 5 | YTA. Your brother didn't ask for advice, he asked for a ride. The fact that you turned it into a lecture tells me you've been waiting for a chance to feel superior to him. | speculation |
| 6 | NTA. He returned the gift the day after he got it. Nobody does that unless they were already planning to be done with the relationship and just wanted the receipt money first. | speculation (I first put reasoning, see below) |
| 7 | ESH. You snapped at her, sure, but she'd been needling you all afternoon. Someone who keeps "joking" about your weight at a family dinner knows exactly what they're doing and is hoping you'll be the one who looks bad when you finally react. | reasoning |
| 8 | YTA. You scheduled the trip on the one weekend she told you she couldn't go. Maybe that was an accident, but booking it anyway after she said no reads like you wanted to go without her and needed an excuse. | speculation |
| 9 | NTA. You gave notice, finished your projects, and left on the agreed date. Your boss calling you "disloyal" is just him trying to guilt you into staying because he never bothered to plan for anyone leaving. | speculation |

What it caught:
- #4 and #6 are almost the same move, take one fact and use a "nobody does that unless" generalization to land on the other person's hidden motive. I had labeled #4 speculation and #6 reasoning, which is contradictory. They are both speculation.
- My speculation definition only said "absent party", but #5 and #8 are inferring the OP's own motive and I still wanted those to count as speculation. So my definition was too narrow.

What I changed:
- Rewrote the speculation definition to cover anyone's unstated motive, the OP or the other person.
- Sharpened the reasoning vs speculation rule into a main point test: if the main point is what happened and whether it was justified it is reasoning, if the main point is why someone secretly did something it is speculation, even when it starts from a real fact.

---

> _Update this file before starting any stretch feature._
