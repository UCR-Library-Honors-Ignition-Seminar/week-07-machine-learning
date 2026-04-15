# Week 7 Assignment: Machine Learning & Creative AI

**Due:** End of Week 7 | **Late policy:** 20% deduction; accepted through end of Week 8

---

## Before You Start

Read the **Background** section in [README.md](README.md) and look at the [Gender Shades](http://gendershades.org/) project before your first session. The critical prompt this week is about data decisions — you need to have made some intentional ones to write about them.

---

## Part 1: Train and Break a Model (Session 1)

Using [Teachable Machine](https://teachablemachine.withgoogle.com):

1. Train an image classifier with at least **2 classes** and at least **50 samples per class**
2. Test it under normal conditions
3. **Deliberately try to break it** — change the angle, background, lighting, or show it something visually similar from the other class
4. Screenshot your training interface
5. Fill in `training-notes.md` (see below)

This step is required even if you use a pretrained model for Part 2 — the training experience is what makes the reflection meaningful.

---

## Part 2: ml5.js + p5.js Sketch

Build an interactive sketch that uses machine learning as a creative material.

**Requirements:**
- Uses **ml5.js** with one of:
  - The model you trained in Teachable Machine (export from TM → load in ml5)
  - A pretrained ml5 model: MobileNet, PoseNet, handpose, or facemesh
- The model's output **drives something visual or textual** — not just a label displayed as text
- Responds to **live input** (webcam)
- Add a comment at the top of your code: what does it do, and what model does it use?

**To submit:**
- Share via p5.js Web Editor link → save in `links.md`, OR
- Download and upload files to your repo

**Step-by-step help:** [INSTRUCTIONS-ML5.md](INSTRUCTIONS-ML5.md)

---

## training-notes.md

Create this file and answer the following (~100 words total):

1. What did you train your model to distinguish?
2. What data did you collect — how many samples, under what conditions?
3. What did you deliberately leave out or not vary?
4. What broke your model, and why do you think it failed there?

---

## Reflection

Write a **~300-word reflection** in `reflection.md` using the [reflection template](reflection-template.md).

**This week's critical prompt (Part 3 of the template):**

> *What did you include in your training data? What did you leave out? What might a model trained on your data misunderstand — and what does that tell you about how ML systems work in the real world?*

Connect your own training experience to a real-world ML system. If you left out certain lighting conditions, certain angles, certain people — what does that mean at scale?

---

## Submission

1. Add your sketch files or `links.md` with share link
2. Add your Teachable Machine training screenshot
3. Create `training-notes.md`
4. Create `reflection.md` using the reflection template
5. Commit your changes and submit your repo URL on Canvas

---

## Grading

| Criterion | Points |
|-----------|--------|
| Teachable Machine model trained + screenshot included | 0.5 |
| ml5.js sketch runs and responds to live input | 1.5 |
| Model output drives something beyond a text label | 0.5 |
| `training-notes.md` documents data choices and failures | 0.5 |
| Reflection: all 4 sections, ~300 words | 1 |
| Reflection connects personal data decisions to real-world ML implications | 0.5 |
| **Total** | **5** |
