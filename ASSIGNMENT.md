# Week 7 Assignment: Machine Learning & Creative AI

**Due:** End of Week 7 | **Late policy:** 20% deduction; accepted through end of Week 8

---

## Before You Start

Read the **Background** section in [README.md](README.md) and look at the [Gender Shades](http://gendershades.org/) project before your first session. The critical prompt this week is about data decisions — you need to have made some intentional ones to write about them.

---

## Part 1: Train and Break a Model (Session 1)

Using [Teachable Machine](https://teachablemachine.withgoogle.com/train):

1. Choose **2–3 categories** to distinguish (art styles, architectural styles, animal species, etc.)
2. Collect at least **10 images per class** from Google Images, [WikiArt](https://www.wikiart.org), or another image source — save them organized by category
3. In Teachable Machine: upload your images under each class, then click **"Train Model"**
4. Test with new images the model has not seen — upload edge cases and ambiguous examples to see where it fails
5. Screenshot your training interface showing your classes and sample count
6. Fill in `training-notes.md` (see below)

This step is required even if you use a pretrained model for Part 2 — the decisions you make collecting and labeling data are what make the reflection meaningful.

---

## Part 2: ml5.js + p5.js Sketch

Build an interactive sketch that uses machine learning as a creative material.

**Requirements:**
- Uses **ml5.js** with one of:
  - The model you trained in Teachable Machine (export from TM → load in ml5)
  - A pretrained ml5 model: MobileNet, PoseNet, handpose, or facemesh
- The model's output **drives something visual or textual** — not just a label displayed as text
- Responds to **live webcam input**
- Add a comment at the top of your code: what does it do, and what model does it use?

**To submit:**
1. Click **File > Save** in the p5.js editor (account required)
2. Click **File > Share** → copy the "Sketch" link
3. In your GitHub repo: click **"Add file"** → **"Create new file"** → name it `links.md` → paste the link → click **"Commit changes"**

**Step-by-step help:** [INSTRUCTIONS-ML5.md](INSTRUCTIONS-ML5.md)

---

## training-notes.md

In your GitHub repo, click **"Add file"** → **"Create new file"** → name it `training-notes.md`. Answer the following (~100 words total):

1. What categories did you train your model to distinguish?
2. Where did you collect your images — what sources, and roughly how many per class?
3. What did you deliberately leave out or not vary in your dataset?
4. What broke your model when you tested it, and why do you think it failed there?

---

## Reflection

Write a **~300-word reflection** by editing `reflection.md` in your repo using the [reflection template](reflection-template.md).

**This week's critical prompt (Part 3 of the template):**

> *What did you include in your training data? What did you leave out? What might a model trained on your data misunderstand — and what does that tell you about how ML systems work in the real world?*

Connect your own data collection decisions to a real-world ML system. If your dataset came mostly from one source, or over-represented one visual style — what does that mean at scale?

---

## Submission Checklist

Before submitting, confirm your repo contains:

- [ ] `links.md` with your p5.js sketch share link
- [ ] Screenshot of your Teachable Machine training interface
- [ ] `training-notes.md` with all 4 questions answered
- [ ] `reflection.md` with all 4 sections filled in

Then paste your GitHub repo URL into the Canvas assignment and submit.

---

## Grading

| Criterion | Points |
|-----------|--------|
| Teachable Machine model trained + training screenshot included | 0.5 |
| ml5.js sketch runs and responds to live webcam input | 1.5 |
| Model output drives something beyond a text label | 0.5 |
| `training-notes.md` documents data collection choices and failures | 0.5 |
| Reflection: all 4 sections, ~300 words | 1 |
| Reflection connects personal data decisions to real-world ML implications | 0.5 |
| **Total** | **5** |
