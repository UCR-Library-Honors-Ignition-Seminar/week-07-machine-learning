# In-Class Activities: Machine Learning

These activities are designed to be done during class — some individually, some in pairs. They are not graded on their own, but what you notice and experience here will feed directly into your reflection.

---

## Session 1 Activities — Teachable Machine

### Activity 1: Train an Intentionally Biased Model (~20 min)

Your goal is to train a model that "fails" in a predictable way — on purpose.

1. Go to [Teachable Machine](https://teachablemachine.withgoogle.com/train) and start a new **Image Project → Standard image model**
2. Choose a 2-class category — for example:
   - "cat" — but only upload images of orange cats for Class 1 and only black cats for Class 2
   - "chair" — office chairs for Class 1, outdoor/lawn chairs for Class 2
   - "landscape" — paintings of landscapes vs. photographs of landscapes
3. Collect at least 10 images per class from Google Images or [WikiArt](https://www.wikiart.org), or other online sources (pay attention to copyrights and licenses)
4. Train the model, then test it with images similar to your class — upload a grey cat, a rocking chair, an aerial photograph

**As you work, think about:**
- Does the model learn the category, or does it learn the visual patterns you happened to show it?
- If you trained "chair" on only office chairs, is the model wrong when it fails on a lawn chair — or is it doing exactly what you told it to do?

---

### Activity 2: Group Discussion (~15 min)

Do this after everyone has trained a model.

1. Form a a group with at least two members (you can move around)e a
2. Discuss in groups what you have observed in Activity 1 and document the discussion
3. Select a group member to share your discussion with the class

**As you discuss:**
- What did your model learn that you didn't intend?
- How is "learning" for machines different from for humans?

---

## Session 2 Activities — ml5 + p5

### Activity 3: MobileNet Bestiary (~15 min, before coding)

Before writing any code, run the MobileNet starter from [README.md](README.md) and spend 10 minutes pointing your webcam at objects around the room.

In your notes, write down every label that appears. Then answer:

- What categories does MobileNet seem to know well?
- What is missing or misidentified?
- What does the list of labels tell you about where this model came from and who made it?

MobileNet was trained on [ImageNet](https://www.image-net.org/) — a dataset assembled mostly from Western internet images in the early 2010s. You are reading that history backwards through its labels.

---

### Activity 4: Same Model, Different Response (~30 min)

Everyone starts with the **identical MobileNet starter** from README. Set a 20-minute timer.

Your goal: make the model's output drive something completely different in `draw()` — not just change a color, but make it do something unexpected or expressive.

Some starting points if you are stuck:
- Make text appear that responds to the label — not the label itself, but something the label makes you think of
- Make the canvas react differently to high-confidence vs. low-confidence readings
- Let the label trigger a change that accumulates over time instead of resetting every frame (remove `background()` from `draw()`)

**When the timer ends:** everyone shares their screen for 60 seconds with no explanation — just show it. Same input data, different outputs. Notice the range.

**After sharing, discuss:**
- What choices did you make about what the model's output *means*?
- Did the model's behavior (its weird labels, its confidence scores) push your sketch in a direction you didn't expect?

---

### Activity 5: Pose Portrait (~30 min, PoseNet)

Use the PoseNet starter from [README.md](README.md). In 20 minutes, make your body position create or change something on screen — not just track a dot, but make the movement mean something.

**Some directions to consider:**
- Your wrist draws something as you move — what does it draw?
- The distance between two keypoints (e.g., left and right wrist) controls a property of the canvas
- Raising your hands vs. lowering them changes the mood or color of the scene
- Your nose position triggers different words or sounds

**When the timer ends:** everyone performs their sketch for 30 seconds — stand in front of your screen, move, let the sketch respond. No explanation needed; the work speaks.

**Afterward:**
- Did the model's keypoints feel like an accurate representation of your body?
- What could you not express because of what PoseNet tracks — and what does that limitation mean?
