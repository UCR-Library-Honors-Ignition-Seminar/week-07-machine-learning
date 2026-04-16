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
- Melanie Mitchell, *Artificial Intelligence: A Guide for Thinking Humans* — a clear, non-technical account of what AI systems can and cannot do, and why; directly relevant to understanding Teachable Machine's limitations
- Kate Crawford, *Atlas of AI* — on the physical, social, and political infrastructure behind machine learning systems
- Mimi Onuoha, [*A People's Guide to AI*](https://mimionuoha.com/a-peoples-guide-to-ai) — free accessible zine-style introduction to AI for non-specialists

---

## Assignment

See [ASSIGNMENT.md](ASSIGNMENT.md) for full requirements, options, and the reflection prompt.
