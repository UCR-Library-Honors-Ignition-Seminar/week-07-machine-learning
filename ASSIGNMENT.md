# Week 7 Assignment: Machine Learning & Creative AI

**Due:** End of Week 7 | **Late policy:** 20% deduction; accepted through end of Week 8

---

## Before You Start

Read the **Background** section in [README.md](README.md) and look at the [Gender Shades](http://gendershades.org/) project before your first session. The critical prompt this week is about data decisions — you need to have made some intentional ones to write about them.

---

## Part 1: Train and Break a Model (Session 1)

Using [Teachable Machine](https://teachablemachine.withgoogle.com/train):

1. Choose **2–3 categories** to distinguish (art styles, architectural styles, animal species, etc.)
2. Collect at least **30–50 images per class** from Google Images, [WikiArt](https://www.wikiart.org), or another image source — organize them into folders by category before uploading
3. In Teachable Machine: upload your images under each class, then click **"Train Model"**
4. Test with new images the model has not seen — upload edge cases and ambiguous examples to see where it fails
5. Take screenshots as you go — of your training interface, test results, and anything interesting that comes up

**How to upload your screenshots to GitHub:**
1. In your GitHub Classroom repo, click **"Add file"** → **"Upload files"**
2. Drag all your screenshots into the upload box at once
3. In the filename field that appears, type `screenshots/` before each filename to create a folder — GitHub will create it automatically
4. Click **"Commit changes"**

> **Tip:** Give your screenshots descriptive names before uploading so you can tell them apart later — for example `training-classes.png`, `test-failure.png`, `confusion-matrix.png`.

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

## Reflection

Write a **~300-word reflection** by editing `reflection.md` in your repo using the [reflection template](reflection-template.md).

The template this week has **5 sections**, including a **Training Data Log** (Section 2) that replaces a separate notes file — answer the data questions there, then the critical prompt (Section 4) asks you to connect those decisions to real-world ML implications.

**This week's critical prompt (Section 4 of the template):**

> *What did you include in your training data? What did you leave out? What might a model trained on your data misunderstand — and what does that tell you about how ML systems work in the real world?*

Connect your own data collection decisions to a real-world ML system. If your dataset came mostly from one source, or over-represented one visual style — what does that mean at scale?

---

## Submission Checklist

Before submitting, confirm your repo contains:

- [ ] `screenshots/` folder with at least one screenshot of your Teachable Machine training interface
- [ ] `links.md` — your p5.js sketch share link
- [ ] `reflection.md` — all 5 sections filled in (including the Training Data Log in Section 2)

Then paste your GitHub repo URL into the Canvas assignment and submit.

---

## Grading

| Criterion | Points |
|-----------|--------|
| Teachable Machine model trained + screenshots uploaded | 0.5 |
| ml5.js sketch runs and responds to live webcam input | 1.5 |
| Model output drives something beyond a text label | 0.5 |
| Reflection Section 2 (Training Data Log): data choices and failures documented | 0.5 |
| Reflection: all 5 sections, ~300 words | 1 |
| Reflection connects personal data decisions to real-world ML implications | 0.5 |
| **Total** | **5** |
