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

1. Form a group with at least two members
2. Discuss what you observed in Activity 1 and document the discussion
3. Select a group member to share your discussion with the class

**As you discuss:**
- What did your model learn that you didn't intend?
- How is "learning" for machines different from for humans?

---

## Session 2 Activities — ml5 + p5

### Activity 3: What Does MobileNet See? (~20 min, no coding required)

Your instructor will have the MobileNet starter running on the projector, or you can run it yourself from [README.md](README.md) — just paste and click Play, no changes needed.

Point the webcam at different objects around the room and watch the labels change. In your notes, write down:
- 5 labels that surprised you
- 2 objects the model got completely wrong — what did it say instead?
- 1 object it seemed confident about

Then discuss as a class:

- What categories does MobileNet seem to know well? What is missing?
- MobileNet was trained on [ImageNet](https://www.image-net.org/) — a dataset assembled mostly from Western internet images in the early 2010s. Looking at the labels it produces, what can you infer about who collected that data and what their world looked like?
- When a facial recognition system is trained on a similarly skewed dataset and then used in a hiring algorithm or a police system — what happens?

---

### Activity 4: Change One Thing (~20 min)

Get the MobileNet starter running (paste the code from [README.md](README.md) into `sketch.js` and click Play). Once it is working, make **one small change** to the `draw()` function — pick any of these:

- Change the numbers in `background(r, 50, b)` to different values and see how the colors shift
- Change `textSize(20)` to a much larger or smaller number
- Change `rect(0, height - 60, width, 60)` — try making the bar taller, or moving it to the top

You do not need to understand the whole sketch. The goal is to change one thing, see what happens, and notice that you can affect how the model's output appears on screen.

**After everyone has tried something, discuss:**
- What did you change, and what happened?
- Have you seen apps or devices that use AI to label or detect something? What are they? Does this exercise change your perception/understanding of those applications?
