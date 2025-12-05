---
title: "Learning ML by Building a Tiny Password Strength Classifier"
date: 2025-11-19
description: "How I stopped overthinking and finally built a simple ML model to classify password strength"
author: "Arsh Imtiaz"
tags: ["machine-learning", "cybersecurity", "passwords"]
---
I finally sat down and built a machine learning model in [**Jupyter Notebook**](https://jupyter.org/) that actually does something cybersecurity related. Not a big fancy neural network. Not a GPT clone. Just a tiny password strength classifier that helped me understand the full ML pipeline without frying my brain.

![Jupyter Notebook](/posts/my-first-ml-model/jupyter_password_ml.png)

This whole thing started because I kept telling myself I would learn ML one day. And one day never comes when you wait for the perfect idea. So I forced myself to build something so stupid simple that I couldn't run away from it.

Turns out, that worked.

---
## What I wanted to build
I wanted a model that takes a password and predicts whether it is strong or weak based on its structure. Nothing about leaks, entropy, breached databases or cracking times. Just pure structural features, built on Jupyter Notebook.

![Empty Notebook](/posts/my-first-ml-model/empty_notebook.png)

I kept it simple and picked four things to analyse:

- length
- uppercase letters
- digits
- symbols

The idea was to convert each password into a set of **numerical features** like:

``length, has_uppercase, has_digit, has_symbol``

So a password like ``Abc123!@`` becomes something like ``8, 1, 1, 1``.

---
## Creating rules that didn't fight me
This was the hardest part. Not the ML. Not the code. Just defining what I believe a strong password is.

At first I made the rule way too strict and then changed it repeatedly. That made the dataset contradictory and the model learned nonsense. Eventually I locked in what actually made sense:

- Password must be at least 8 characters long
- And it must have at least 2 out of these 3:
  - uppercase
  - digit
  - symbol

So something short like ``8B$`` is weak even though it has good complexity. And something long like ``averystrongpassword`` is weak because it has no variety. Finally, the rules aligned with my intuition.

---
## Writing the labeler function
I wrote a tiny function that checks each password and turns it into a 0 or 1 based on the rules.

```python
def is_strong(pw: str) -> int:  
	length_ok = len(pw) >= 8  
	has_upper = any(c.isupper() for c in pw)  
	has_digit = any(c.isdigit() for c in pw)  
	has_symbol = bool(re.search(r'[^A-Za-z0-9]', pw))
	
	if not length_ok:
	    return 0

	complexity_score = (
	    int(has_upper) +
	    int(has_digit) +
	    int(has_symbol)
	)
	
	return int(complexity_score >= 2)
```

Once I had this, I relabeled my dataset and everything started behaving predictably.

![Labeller](/posts/my-first-ml-model/labeller.png)

---
## Turning passwords into features

My extractor function was tiny too:
```python
def extract_features(pw):  
	length = len(pw)  
	has_upper = int(any(c.isupper() for c in pw))  
	has_digit = int(any(c.isdigit() for c in pw))  
	has_symbol = int(bool(re.search(r'[^A-Za-z0-9]', pw)))  
	
	return [length, has_upper, has_digit, has_symbol]
```

![Extract Features](/posts/my-first-ml-model/extract_features.png)

This gave me clean numerical data I could feed into scikit learn.

## Training the model
After that, the ML part was almost boring. In a good way.

```python
model = LogisticRegression()  
model.fit(X_train, y_train)
```

I evaluated it with ``classification_report`` and it performed exactly how you'd expect on such a tiny dataset. Not perfect, but good enough to prove that:

- my labels made sense
- my features were consistent
- the model actually learned the pattern instead of memorising random junk

![Training](/posts/my-first-ml-model/training.png)

## What I learned
![Me](/posts/my-first-ml-model/cat-soldier.gif)


Honestly, the biggest lesson wasn't about ML. It was about myself.

- I overthink everything when I try to learn something new
- Simple models are the best place to start
- Jupyter notebooks make experimentation painless
- ML is not scary once you run a full cycle end to end
- A small dataset is actually a blessing when you're trying to understand the process

This little password strength classifier is nowhere near real world use cases, but it taught me how ML actually works instead of how it works in theory.

## What's next
I might expand it with more features like:
- checking for dictionary words - because [seclists](https://github.com/danielmiessler/SecLists) is very beefy
- repeated patterns
- keyboard adjacency
- entropy approximations

But I'll do it one step at a time. The whole point of this exercise was to stop trying to build the final boss on day one.

And honestly, making this tiny password checker did more for my ML understanding than any tutorial ever has.

