# Week 7: Machine Learning Basics & Creative AI

**Instructor:** Jing

---

## Learning Objectives

- Explain what a machine learning model "learns" and from what data
- Train an image classifier using Teachable Machine
- Intentionally break your model to understand its limitations
- Connect an ml5.js model to a p5.js sketch
- Link dataset decisions to ethical and aesthetic outcomes

---

## Introduction: What Does a Model Learn?

A machine learning model doesn't understand the world the way you do. It learns patterns from examples you show it. Show it 200 photos of cats and 200 photos of dogs, and it finds correlations — not because it understands "cat," but because it detected patterns in your examples.

**Your training data is your argument.** Whatever you include, exclude, or mislabel shapes what the model "sees." A facial recognition system trained mostly on light-skinned faces will perform poorly on dark-skinned faces — not a technical failure, but a data choice.

---

## Session 1 (Day 1): Teachable Machine & ML Foundations

### Teachable Machine (browser, no code)
[teachablemachine.withgoogle.com](https://teachablemachine.withgoogle.com)

1. Choose "Image Project" > "Standard image model"
2. Add 2-3 classes
3. Click "Webcam" under each class and collect 50-100 samples
4. Click "Train Model" (30-60 seconds)
5. Test live in the preview window

**In-class activity: Train and break your model**
1. Train a classifier (gestures, objects, facial expressions)
2. Test under normal conditions
3. Deliberately break it: wrong angle, new background, similar-looking objects
4. Note what failed and why

### Key concepts
| Term | Meaning |
|------|---------|
| **Training data** | Examples you show the model |
| **Class** | A category the model learns to distinguish |
| **Overfitting** | Model memorizes training data, fails on new examples |
| **Bias** | Training data systematically over/under-represents something |

---

## Session 2 (Day 2): ml5.js + p5.js Creative Sketch

**ml5.js** is a friendly machine learning library built on top of p5.js.

### Option A: MobileNet image classifier (pretrained)
```javascript
let classifier;
let video;
let label = "loading...";

function setup() {
  createCanvas(640, 480);
  video = createCapture(VIDEO);
  video.hide();
  classifier = ml5.imageClassifier('MobileNet', video, modelReady);
}
function modelReady() {
  classifier.classify(gotResult);
}
function gotResult(results) {
  label = results[0].label;
  classifier.classify(gotResult);
}
function draw() {
  image(video, 0, 0);
  textSize(24); fill(255);
  text(label, 10, height - 20);
}
```

### Option B: Your Teachable Machine model
1. In Teachable Machine > "Export Model" > "Tensorflow.js" > "Upload my model"
2. Copy the model URL
3. Use: `let classifier = ml5.imageClassifier('YOUR_MODEL_URL_HERE', modelReady);`

### Option C: PoseNet (body tracking)
```javascript
let poseNet, poses = [];
function setup() {
  createCanvas(640, 480);
  let video = createCapture(VIDEO);
  video.hide();
  poseNet = ml5.poseNet(video, () => console.log('model loaded'));
  poseNet.on('pose', results => { poses = results; });
}
function draw() {
  background(0);
  if (poses.length > 0) {
    let nose = poses[0].pose.nose;
    fill(255, 0, 0);
    circle(nose.x, nose.y, 30);
  }
}
```

---

## Resources

- Teachable Machine: [teachablemachine.withgoogle.com](https://teachablemachine.withgoogle.com)
- ml5.js: [ml5js.org](https://ml5js.org) | Learn: [learn.ml5js.org](https://learn.ml5js.org)
- p5.js editor: [editor.p5js.org](https://editor.p5js.org)
- ML for Artists: [ml4a.github.io](https://ml4a.github.io)

## Project & Assignment

See [ASSIGNMENT.md](ASSIGNMENT.md) for full requirements.
