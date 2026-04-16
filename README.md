# Week 7: Machine Learning Basics & Creative AI

**Instructor:** Jing | **Weeks 5–8 format:** Critical/Creative Reflection

---

## Learning Objectives

- Explain what a machine learning model "learns" and from what data
- Train an image classifier using Teachable Machine and deliberately break it
- Connect an ml5.js model to a p5.js sketch
- Link dataset decisions to ethical and aesthetic outcomes

---

## Background: What Does a Model Actually Learn?

A machine learning model does not understand the world the way you do. It finds statistical patterns in examples you show it. Show it 200 photos of cats and 200 of dogs, and it learns which pixel patterns tend to appear with which label — not because it understands "cat," but because it detected correlations in your data.

**Your training data is your argument.** Whatever you include, exclude, or mislabel shapes what the model "sees." This has real consequences:

- A hiring algorithm trained on resumes from a historically male field will score women lower — not because it was told to, but because the data reflected existing bias
- A medical image classifier trained mostly on light-skinned patients performs worse on dark-skinned patients
- A content moderation model trained on English text struggles with other languages — not because it was designed to, but because the training corpus was imbalanced

These are not edge cases or bugs. They are the predictable result of decisions made when assembling training data. This week you will make those decisions yourself — on a small scale — and feel what they mean.

### Recommended viewing before class

- [Gender Shades project](http://gendershades.org/) — How well do IBM, Microsoft, and Face++ AI services guess the gender of a face?
- [ml4a — Machine Learning for Artists](https://ml4a.github.io/ml4a/) — Chapters, guides, demos, code, and more. Explore, read, experiment based on your interest
---

## Session 1 (Day 1): Teachable Machine

### What it is

[Teachable Machine](https://teachablemachine.withgoogle.com) is a browser-based tool from Google that lets you train an image, sound, or pose classifier using your webcam — no code required. It is a simplified interface to a real machine learning process: you are training an actual neural network.

### Train a classifier

1. Go to [teachablemachine.withgoogle.com](https://teachablemachine.withgoogle.com)
2. Click **"Get Started"** → **"Image Project"** → **"Standard image model"**
3. You will see two classes (Class 1, Class 2) — rename them to what you are distinguishing
4. Click **"Webcam"** under each class and hold objects/make gestures in front of your camera — collect at least **50 samples per class**
5. Click **"Train Model"** — takes 30–60 seconds
6. Test in the live preview panel on the right

**Ideas for what to train:**
- Two hand gestures (thumbs up vs. open palm)
- Two objects on your desk
- Two facial expressions
- "Dark background" vs. "light background" — to show the model learning the wrong thing

### Break your model intentionally

After training, try to fool it:

| Test | What you are probing |
|------|---------------------|
| Show the object at a new angle | Did the model learn shape or just your specific view? |
| Change the background | Did it learn the object or the setting behind it? |
| Show something visually similar from the other class | Where is the boundary? |
| Use the model in bad lighting | How fragile is it? |

**Write down what fails and why** — this is your `training-notes.md` material and the foundation of your reflection.

### Key vocabulary

| Term | Meaning |
|------|---------|
| **Training data** | The examples you show the model during training |
| **Class** | A category the model learns to distinguish |
| **Confidence score** | How certain the model is (0–100%) — high confidence does not mean correct |
| **Overfitting** | Model memorizes training examples but fails on anything new |
| **Bias** | Training data systematically over- or under-represents something, skewing outputs |

---

## Session 2 (Day 2): ml5.js + p5.js

**ml5.js** is a JavaScript library that wraps complex machine learning models in beginner-friendly functions. It is designed to work alongside p5.js — you can connect a model's output directly to what appears on your canvas.

See **[INSTRUCTIONS-ML5.md](INSTRUCTIONS-ML5.md)** for full setup and code walkthroughs.

### Three starting points

**Option A — MobileNet (pretrained image classifier)**
MobileNet is trained on 1,000 everyday object categories. Point your webcam at things and see what it recognizes — or misrecognizes.

```javascript
let classifier, video, label = "loading...";

function setup() {
  createCanvas(640, 480);
  video = createCapture(VIDEO);
  video.hide();
  classifier = ml5.imageClassifier("MobileNet", video, () => {
    classifier.classify(gotResult);
  });
}
function gotResult(results) {
  label = results[0].label;
  classifier.classify(gotResult);
}
function draw() {
  image(video, 0, 0);
  fill(255); noStroke(); textSize(24);
  text(label, 10, height - 20);
}
```

**Option B — Your Teachable Machine model**
Export your trained model from Teachable Machine and load it in ml5.js:

1. In Teachable Machine: click **"Export Model"** → **"Tensorflow.js"** → **"Upload (shareable link)"**
2. Copy the model URL
3. Replace `"MobileNet"` in the code above with your model URL

**Option C — PoseNet (body tracking)**
PoseNet detects 17 body keypoints (nose, shoulders, wrists, hips, etc.) from webcam video. Use body position to control what appears on screen.

```javascript
let poseNet, poses = [];

function setup() {
  createCanvas(640, 480);
  let video = createCapture(VIDEO);
  video.hide();
  poseNet = ml5.poseNet(video, () => console.log("model ready"));
  poseNet.on("pose", results => { poses = results; });
}
function draw() {
  background(0);
  if (poses.length > 0) {
    let nose = poses[0].pose.nose;
    fill(255, 100, 0); noStroke();
    circle(nose.x, nose.y, 40);
    // try: wrists, ankles, shoulders — poses[0].pose.leftWrist, etc.
  }
}
```

### Make the output do something creative

A label appearing as text is a starting point, not a finished piece. Once you have a model running, connect its output to something:

| Model output | Creative use |
|-------------|-------------|
| Classification label | Change background color, trigger a word, switch between two images |
| Confidence score (0–1) | Control opacity, size, speed of something on screen |
| Pose keypoint (x, y) | Draw at that position; use distance between two points to control a variable |
| Your Teachable Machine class | Display different text, play different sounds, change the canvas entirely |

---

## Resources

### Tools
- [Teachable Machine](https://teachablemachine.withgoogle.com) — browser-based, no account needed
- [ml5.js](https://ml5js.org) — library reference and examples
- [ml5.js Learn](https://learn.ml5js.org) — official tutorials
- [p5.js Web Editor](https://editor.p5js.org) — where you write and run your sketch

### Examples and Context
- [ml4a — Machine Learning for Artists](https://ml4a.github.io/ml4a/) — Gene Kogan's visual introduction to ML for creative practitioners
- [Gender Shades](http://gendershades.org/) — Joy Buolamwini's facial recognition bias research
- [*Coded Bias* documentary](https://www.codedbias.com/) — film following the Gender Shades research
- [AI Experiment with Googl's Teachable Machine examples](https://teachablemachine.withgoogle.com/faq) — what others have trained

### Further Reading
- Kate Crawford, [*Atlas of AI*](https://www.penguinrandomhouse.com/books/621449/atlas-of-ai-by-kate-crawford/) — on the physical and social infrastructure behind machine learning systems
- Mimi Onuoha, [*A People's Guide to AI*](https://mimionuoha.com/a-peoples-guide-to-ai) — free, accessible zine-style guide to understanding AI for non-specialists

---

## Assignment

See [ASSIGNMENT.md](ASSIGNMENT.md) for full requirements, options, and the reflection prompt.
