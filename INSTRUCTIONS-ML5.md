# Step-by-Step: Building an ml5.js + p5.js Sketch

**Time needed:** 45–60 minutes | **Requires:** webcam | **No downloads required**

---

## What is ml5.js?

ml5.js is a JavaScript library that makes machine learning models accessible from a browser sketch. It sits on top of p5.js — you use both together. ml5 loads the model; p5 draws the output.

You do not need to understand how the model works internally. You need to know: what does it take as input, and what does it give back?

---

## Step 1: Open the p5.js Editor

Go to [editor.p5js.org](https://editor.p5js.org) and sign in (required to save your work).

You need to add ml5.js to the editor. In the editor:
1. Click the **"Sketch Files"** arrow (top left)
2. Open `index.html`
3. The file already has p5 script tags — **do not delete them.** Add one line after the existing scripts:

```html
<head>
  <script src="https://cdn.jsdelivr.net/npm/p5@1.11.13/lib/p5.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/p5@1.11.13/lib/addons/p5.sound.min.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/ml5@0.12.2/dist/ml5.min.js"></script>
</head>
```

> Use `ml5@0.5.2` — a specific pinned version. Using `ml5@0.5.2` will cause a `classifier.classify is not a function` error because the newest ml5 version changed its API.

---

## Step 2: Allow Camera Access

ml5 uses your webcam as live input. When the sketch runs, your browser will ask for camera permission — click Allow.

Add this to `setup()` to create a video feed:

```javascript
let video;

function setup() {
  createCanvas(640, 480);
  video = createCapture(VIDEO);
  video.hide(); // hide the raw HTML video element; we'll draw it in draw()
}

function draw() {
  image(video, 0, 0, width, height); // draw video to canvas
}
```

Run this first and confirm your camera appears on the canvas before adding ml5.

---

## Step 3: Load a Model

### Option A: MobileNet (pretrained, recognizes 1,000 objects)

MobileNet is trained on 1,000 everyday object categories. Point your webcam at things and see what it recognizes — or misrecognizes.

```javascript
let classifier, video;
let label = "loading model...";
let confidence = 0;

function setup() {
  createCanvas(640, 480);
  video = createCapture(VIDEO);
  video.hide();

  classifier = ml5.imageClassifier("MobileNet", video, modelReady);
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

  fill(0, 0, 0, 150);
  noStroke();
  rect(0, height - 60, width, 60);
  fill(255);
  textSize(20);
  textAlign(LEFT, CENTER);
  text(label + " — " + nf(confidence * 100, 2, 1) + "%", 10, height - 30);
}
```

### Option B: Your Teachable Machine model

After training your model in Session 1:

1. In Teachable Machine, click **"Export Model"**
2. Choose **"Tensorflow.js"** → **"Upload (shareable link)"**
3. Click **"Upload my model"** — you get a URL like `https://teachablemachine.withgoogle.com/models/XXXXXXX/`
4. Replace `"MobileNet"` in the code above with that URL:

```javascript
classifier = ml5.imageClassifier("https://teachablemachine.withgoogle.com/models/XXXXXXX/", video, modelReady);
```

Everything else stays the same. Your model now classifies live webcam input using the categories you trained on.

### Option C: PoseNet (detects 17 body keypoints)

PoseNet detects body keypoints (nose, shoulders, wrists, hips, etc.) from webcam video in real time.

```javascript
let poseNet, poses = [];
let video;

function setup() {
  createCanvas(640, 480);
  video = createCapture(VIDEO);
  video.hide();

  poseNet = ml5.poseNet(video, modelReady);
  poseNet.on("pose", function(results) {
    poses = results;
  });
}

function modelReady() {
  console.log("PoseNet ready!");
}

function draw() {
  image(video, 0, 0, width, height);

  if (poses.length > 0) {
    let pose = poses[0].pose;

    fill(255, 0, 0);
    noStroke();
    circle(pose.nose.x, pose.nose.y, 30);

    // other available keypoints:
    // pose.leftEye, pose.rightEye
    // pose.leftShoulder, pose.rightShoulder
    // pose.leftWrist, pose.rightWrist
    // pose.leftHip, pose.rightHip
    // pose.leftAnkle, pose.rightAnkle
  }
}
```

---

## Step 4: Make the Output Do Something

A label in text is a starting point. Connect the model's output to something visual or expressive:

**Label → background color:**
```javascript
function draw() {
  if (label.includes("person")) {
    background(50, 50, 200);
  } else {
    background(200, 50, 50);
  }
  image(video, 0, 0, 320, 240);
}
```

**Confidence score → shape size:**
```javascript
function draw() {
  background(0);
  image(video, 0, 0, width, height);
  let size = map(results[0].confidence, 0, 1, 10, 300);
  fill(255, 200, 0, 150);
  noStroke();
  circle(width / 2, height / 2, size);
}
```

**Pose keypoint → drawing position:**
```javascript
function draw() {
  background(0);
  if (poses.length > 0) {
    let wrist = poses[0].pose.rightWrist;
    fill(255, 100, 0, 100);
    noStroke();
    circle(wrist.x, wrist.y, 40);
  }
}
```

| Model output | Creative use ideas |
|-------------|-------------------|
| Classification label | Change background color, trigger a word, switch between images |
| Confidence score (0–1) | Control opacity, size, or speed of something on screen |
| Pose keypoint (x, y) | Draw at that position; measure distance between two points |
| Your Teachable Machine class | Display different text, change the canvas mood entirely |

---

## Step 5: Save and Share

1. Click **File > Save**
2. Click **File > Share** → copy the "Sketch" link
3. In your GitHub repo: click **"Add file"** → **"Create new file"** → name it `links.md` → paste the link → click **"Commit changes"**

---

## Troubleshooting

| Problem | Fix |
|---------|-----|
| "ml5 is not defined" | ml5 script tag is missing or in wrong order in `index.html` |
| Camera permission denied | Refresh the page and click Allow when prompted |
| Model loads but nothing classifies | Make sure `classifier.classify(gotResult)` is called inside `modelReady()` |
| PoseNet runs but no keypoints appear | Check `poses.length > 0` before accessing `poses[0]`; give the model a second to warm up |
| Teachable Machine URL not working | Make sure you clicked "Upload my model" before copying the link — not just "Export" |
| Sketch runs slowly | The video + model is computationally heavy; close other browser tabs |
