# Week 7: Machine Learning Basics & Creative AI

**Instructor:** Jing

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

These are not edge cases or bugs. They are the predictable result of decisions made when assembling training data. This week you will make those decisions yourself — on a small scale — and experience what they mean.

### Recommended viewing before class

- [Gender Shades project](http://gendershades.org/) — How well do IBM, Microsoft, and Face++ AI services guess the gender of a face?
- [ml4a — Machine Learning for Artists](https://ml4a.github.io/ml4a/) — Chapters, guides, demos, code, and more. Explore, read, experiment based on your interest
---

## Session 1 (Day 1): Teachable Machine (Building an Image Classifer)

### What it is

[Teachable Machine](https://teachablemachine.withgoogle.com) is a browser-based tool from Google that lets you train a computer to recognize your own images, sounds, & poses — no code required. It is a simplified interface to a real machine learning process but you are training an actual neural network. Scroll down on the Teacheble Machine's website to view tutorials and example projects.

### Step 1: Choose your categories

Pick **2–3 categories** that are visually distinct but share some similarities — the interesting questions come from the edge cases. Good starting points:

- **Art styles** — Impressionism vs. Cubism (search Google Images or WikiArt)
- **Architectural styles** — Gothic vs. Modernist buildings
- **Book or comic cover styles** — sci-fi covers vs. romance covers
- **Animal species** — two breeds of dog, or dog vs. cat vs. bird
- **Textures** — smooth vs. rough vs. patterned surfaces
- **Landscapes vs. cityscapes**

If you are using your webcam, avoid categories you could only capture with your webcam in one setting/background — you want enough variety in your images that the model learns the category.

### Step 2: Collect images

Find at least **10 images per class** using Google Images, [WikiArt](https://www.wikiart.org), or any image search. Save them to a folder on your computer, organized by category (Experiment with uploading more or less images per class)

**Think about variety as you collect:**
- Different lighting conditions
- Different angles or crops
- Different backgrounds
- Images from different sources

The images you collect *are* your argument about what the category means. If all your "Impressionism" images are Monet water lilies, the model will learn Monet — not Impressionism.

### Step 3: Train your model

1. Go to [teachablemachine.withgoogle.com/train](https://teachablemachine.withgoogle.com/train)
2. Click **"Image Project"** → **"Standard image model"**
3. Rename the classes to match your categories
4. Click **"Upload"** under each class → select your saved images
5. Click **"Train Model"** — takes 30–60 seconds
6. Test in the live preview panel — upload a new image the model has not seen

### Break your model intentionally

After training, try to fool it with new images:

| Test | What you are probing |
|------|---------------------|
| Upload an image that is ambiguous between two classes | Where is the boundary? What does the model do with uncertainty? |
| Upload an image from outside your categories entirely | Does it confidently misclassify it? |
| Upload a low-quality, cropped, or unusual version of a correct category | Did it learn the concept or just the typical presentation? |
| Upload something with the right color palette but wrong subject | Is it classifying by color or by content? |

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

### How the two libraries connect

You already know p5.js from Week 6 — it draws things on a canvas. **ml5.js** is an add-on library that gives p5.js the ability to run machine learning models. The two can work together:

- **p5.js** handles the canvas, the webcam video, and drawing shapes and text
- **ml5.js** handles loading the model and classifying what it sees in the webcam feed
- Every time ml5 produces a result (a label, a confidence score, a body position), you use p5 to draw something based on that result

Think of it as: *ml5 listens, p5 responds.*

### See it in action first

Before writing any code, look at what people have built with ml5 + p5:

- [ml5.js examples gallery](https://docs.ml5js.org/#/reference/overview) — scroll through the examples on the left; each one shows a live demo and the full code
- [The Coding Train ml5 playlist](https://www.youtube.com/playlist?list=PLRqwX-V7Uu6YPSwT06y_AEYTqIwbeam3y) — short videos showing ml5 sketches being built from scratch

**As you look:** What is the model detecting? What does p5 do with that information — what changes on screen?

### Step 1: Set up the p5.js editor with ml5

1. Go to [editor.p5js.org](https://editor.p5js.org) and sign in
2. Click the **arrow icon** below the Play button (top left panel)
3. Click on `index.html` to open it
4. Select **all** the existing content (Cmd+A / Ctrl+A) and replace it with the complete code below:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <script src="https://cdn.jsdelivr.net/npm/p5@1.11.13/lib/p5.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/p5@1.11.13/lib/addons/p5.sound.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/ml5@0.12.2/dist/ml5.min.js"></script>
    <link rel="stylesheet" type="text/css" href="style.css">
    <meta charset="utf-8" />
  </head>
  <body>
    <main>
    </main>
    <script src="sketch.js"></script>
  </body>
</html>
```

> The only addition to the default file is the ml5 line — but replacing the whole file is simpler than finding exactly where to insert it. Use `ml5@0.12.2` specifically — this is the version the starter code is written for.

5. Click back to `sketch.js` — this is where you write your code

### Step 2: Choose a starter sketch and paste it in

Pick **one** of the two options below. Copy the entire code block, select everything in `sketch.js` (Cmd+A / Ctrl+A), and paste to replace it. Then click Play (▶).

---

**Option A — MobileNet: webcam object classifier**

MobileNet is a model pretrained on 1,000 everyday object categories. It reads your webcam and labels what it sees every frame.

```javascript
// ML5 + p5.js starter: MobileNet image classifier
// The model reads your webcam and labels what it sees every 500ms.
// async/await tells the sketch to wait for the model before classifying.

let classifier, video;
let label = "loading model...";
let confidence = 0;

async function setup() {
  createCanvas(640, 480);
  video = createCapture(VIDEO);
  video.hide();
  // wait for MobileNet to load — takes 5–10 seconds
  classifier = await ml5.imageClassifier("MobileNet", video);
  console.log("Model ready!");
  classifyVideo(); // start the classification loop
}

async function classifyVideo() {
  let results = await classifier.classify();
  label = results[0].label;
  confidence = results[0].confidence;
  // wait 500ms before classifying again — responsive but not frantic
  setTimeout(classifyVideo, 500);
}

function draw() {
  image(video, 0, 0, width, height);

  // display the label — THIS IS WHAT YOU MODIFY
  fill(0, 0, 0, 150);
  noStroke();
  rect(0, height - 60, width, 60);
  fill(255);
  textSize(20);
  textAlign(LEFT, CENTER);
  text(label + " — " + nf(confidence * 100, 2, 1) + "%", 10, height - 30);
}
```

> **Note on MobileNet labels:** MobileNet is trained on 1,000 very specific ImageNet categories — it has "ping pong ball" and "desk lamp" but not "person" or "office." Strange labels are normal and are actually useful data: they show what visual patterns the model was trained to find. If it says "ping pong ball" when you're in the frame, something about your background or clothing matched that pattern in the training data. This is worth noting in your `training-notes.md`.

Click Play. Allow camera access. Wait 5–10 seconds for the model to load. You should see your webcam with a label at the bottom.

---

**Option B — PoseNet: body keypoint tracker**

PoseNet detects 17 body positions (nose, shoulders, wrists, hips, ankles) from your webcam in real time.

```javascript
// ML5 + p5.js starter: PoseNet body tracker
// The model finds body keypoints in the webcam feed.
// Use the keypoint positions (x, y) to draw things on screen.

let poseNet, poses = [];
let video;

function setup() {
  createCanvas(640, 480);
  video = createCapture(VIDEO);
  video.hide();
  // load PoseNet; modelReady is called when it finishes loading
  poseNet = ml5.poseNet(video, modelReady);
  poseNet.on("pose", function(results) {
    poses = results; // update poses every time a new detection comes in
  });
}

function modelReady() {
  console.log("PoseNet ready!");
}

function draw() {
  image(video, 0, 0, width, height);

  if (poses.length > 0) {             // only draw if a body is detected
    let pose = poses[0].pose;         // get the first detected person

    // draw a circle at the nose — THIS IS WHAT YOU MODIFY
    fill(255, 100, 0);
    noStroke();
    circle(pose.nose.x, pose.nose.y, 40);

    // other keypoints you can use:
    // pose.leftWrist    pose.rightWrist
    // pose.leftShoulder pose.rightShoulder
    // pose.leftHip      pose.rightHip
    // pose.leftAnkle    pose.rightAnkle
    // pose.leftEye      pose.rightEye
  }
}
```

Click Play. Allow camera access. Stand back so your upper body is visible. You should see an orange circle tracking your nose.

---

### Step 3: Make it do something

Once your starter is running, modify the `draw()` function to make the model output drive something visual. **Do not start from scratch — change one thing at a time.**

**If you chose Option A (MobileNet), try:**

Use the confidence score to shift the background color — higher confidence means warmer color:
```javascript
function draw() {
  // map confidence (0 to 1) to a color shift — always changing, always visible
  let r = map(confidence, 0, 1, 30, 220);
  let b = map(confidence, 0, 1, 220, 30);
  background(r, 50, b);
  image(video, 0, 0, 320, 240);   // smaller video in the corner
  fill(255); textSize(18);
  text(label, 10, 260);
}
```

> MobileNet does not have "person" as a category — it has very specific ImageNet labels like "laptop", "desk lamp", "coffee mug". Using confidence to drive color works reliably no matter what the model returns.

Or use the confidence score to control a shape's size (`confidence` is a global variable updated by `classifyVideo`):
```javascript
function draw() {
  background(20);
  image(video, 0, 0, width, height);
  // confidence is 0 to 1 — map it to a circle diameter
  let diameter = map(confidence, 0, 1, 20, 300);
  fill(255, 200, 0, 150);
  noStroke();
  circle(width / 2, height / 2, diameter);
  fill(255); textSize(16);
  text(label, 10, height - 10);
}
```

**If you chose Option B (PoseNet), try:**

Draw a trail wherever your right wrist goes:
```javascript
function draw() {
  // no background() — so shapes accumulate and leave a trail
  if (poses.length > 0) {
    let wrist = poses[0].pose.rightWrist;
    fill(255, 100, 0, 80);
    noStroke();
    circle(wrist.x, wrist.y, 30);
  }
}
```

Or use wrist height to change the background mood:
```javascript
function draw() {
  if (poses.length > 0) {
    let wrist = poses[0].pose.rightWrist;
    // wrist.y is 0 at top, 480 at bottom — map to a color
    let col = map(wrist.y, 0, height, 255, 0);
    background(0, col, 255 - col);
    image(video, 0, 0, 320, 240);
  }
}
```

**The pattern is always the same:** get a value from ml5 (a label, a confidence number, an x/y position) → use it as a variable in p5 (a color, a size, a position). Start with one substitution, see what it does, then change something else.

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
- Melanie Mitchell, *Artificial Intelligence: A Guide for Thinking Humans* — a clear, non-technical account of what AI systems can and cannot do, and why; directly relevant to understanding Teachable Machine's limitations
- Kate Crawford, *Atlas of AI* — on the physical, social, and political infrastructure behind machine learning systems
- Mimi Onuoha, [*A People's Guide to AI*](https://mimionuoha.com/a-peoples-guide-to-ai) — free accessible zine-style introduction to AI for non-specialists

---

## Assignment

See [ASSIGNMENT.md](ASSIGNMENT.md) for full requirements, options, and the reflection prompt.
