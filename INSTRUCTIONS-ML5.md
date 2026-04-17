# Step-by-Step: Building an ml5.js + p5.js Sketch

**Time needed:** 45–60 minutes | **Requires:** webcam | **No downloads required**

---

## What is ml5.js?

ml5.js is a JavaScript library that makes machine learning models accessible from a browser sketch. It sits on top of p5.js — you use both together. ml5 loads the model; p5 draws the output.

You do not need to understand how the model works internally. You need to know: what does it take as input, and what does it give back?

---

## Step 1: Open the p5.js Editor and Set Up ml5

Go to [editor.p5js.org](https://editor.p5js.org) and sign in (required to save your work).

ml5.js is not included by default — you need to add it to the HTML file:

1. Click the **"Sketch Files"** arrow (top left of the editor)
2. Click **`index.html`** to open it
3. **Select all** (Cmd+A / Ctrl+A) and **paste the following**, replacing everything:

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

> **Do not use the newest ml5** — the latest version changed its API and will cause errors. The version pinned above (`ml5@0.12.2`) is the one that works with these examples.

4. Click back to **`sketch.js`** — this is where you write your code.

---

## Step 2: Choose a Starter Sketch

Pick **one** of the three options below. Copy the entire code block, select everything in `sketch.js` (Cmd+A / Ctrl+A), and paste to replace it. Then click Play (▶) and allow camera access when prompted.

---

### Option A: MobileNet (pretrained — recognizes 1,000 objects)

MobileNet is trained on 1,000 everyday object categories. Point your webcam at things and see what it recognizes — or misrecognizes. Strange labels are normal: they reveal what visual patterns the model actually learned.

```javascript
let classifier, video;
let label = "loading model...";
let confidence = 0;

async function setup() {
  createCanvas(640, 480);
  video = createCapture(VIDEO);
  video.hide();
  classifier = await ml5.imageClassifier("MobileNet", video);
  classifyVideo();
}

async function classifyVideo() {
  let results = await classifier.classify();
  label = results[0].label;
  confidence = results[0].confidence;
  setTimeout(classifyVideo, 500); // wait 500ms before classifying again
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

Click Play. Allow camera access. Wait 5–10 seconds for the model to load. You should see your webcam with a label and confidence percentage at the bottom.

> **Note on MobileNet labels:** MobileNet has no "person" category — it has very specific ImageNet labels like "laptop", "desk lamp", "coffee mug". If it says "ping pong ball" when you're in frame, something about your background or clothing matched that training pattern. This is worth noting in the Training Data Log section of your `reflection.md`.

---

### Option B: PoseNet (detects 17 body keypoints)

PoseNet detects body keypoints — nose, shoulders, wrists, hips, etc. — from live webcam video. Unlike MobileNet, it gives you x/y coordinates for each keypoint, not labels.

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
    let pose = poses[0].pose;         // get the first detected person

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

Click Play. Allow camera access. Stand back so your upper body is visible. You should see an orange circle tracking your nose.

---

### Option C: Your Teachable Machine Model

Use the model you trained in Session 1, with the exact class names you defined.

**First, export your model from Teachable Machine:**
1. In Teachable Machine, click **"Export Model"**
2. Choose **"Tensorflow.js"** → **"Upload (shareable link)"**
3. Click **"Upload my model"** — you get a URL like `https://teachablemachine.withgoogle.com/models/XXXXXXX/`
4. Copy that URL

**Then paste this code into `sketch.js`**, replacing `YOUR_MODEL_URL` with the URL you copied:

```javascript
let classifier, video;
let label = "loading model...";
let confidence = 0;

const MODEL_URL = "https://teachablemachine.withgoogle.com/models/XXXXXXX/";

async function setup() {
  createCanvas(640, 480);
  video = createCapture(VIDEO);
  video.hide();
  classifier = await ml5.imageClassifier(MODEL_URL, video);
  classifyVideo();
}

async function classifyVideo() {
  let results = await classifier.classify();
  label = results[0].label;
  confidence = results[0].confidence;
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

Click Play. Allow camera access. Point your webcam at one of the categories you trained on. You should see your own class labels appearing at the bottom.

---

## Step 3: Make the Output Do Something

A label in a text box is a starting point. Use the model's output to drive something visual.

**If you chose Option A (MobileNet):**

Since MobileNet's class names are unpredictable, confidence score is the most reliable variable to work with:

```javascript
// Confidence → background color (warmer = more confident)
function draw() {
  let r = map(confidence, 0, 1, 30, 220);
  let b = map(confidence, 0, 1, 220, 30);
  background(r, 50, b);
  image(video, 0, 0, 320, 240);
  fill(255);
  textSize(18);
  text(label, 10, 260);
}
```

```javascript
// Confidence → circle size
function draw() {
  background(20);
  image(video, 0, 0, width, height);
  let diameter = map(confidence, 0, 1, 20, 300);
  fill(255, 200, 0, 150);
  noStroke();
  circle(width / 2, height / 2, diameter);
}
```

**If you chose Option B (PoseNet):**

Keypoints give you x/y positions — use them as drawing coordinates or measure distance between them:

```javascript
// Draw a trail at the right wrist
function draw() {
  // no background() — shapes accumulate as you move
  if (poses.length > 0) {
    let wrist = poses[0].pose.rightWrist;
    fill(255, 100, 0, 100);
    noStroke();
    circle(wrist.x, wrist.y, 40);
  }
}
```

```javascript
// Wrist height → background mood
function draw() {
  if (poses.length > 0) {
    let wrist = poses[0].pose.rightWrist;
    let col = map(wrist.y, 0, height, 0, 255);
    background(0, col, 255 - col);
  }
}
```

**If you chose Option C (Teachable Machine):**

Because you named the classes yourself, you can use `label` directly to branch on your own category names:

```javascript
// Different background color per class
function draw() {
  if (label === "YourClassOne") {
    background(255, 200, 100);   // warm
  } else if (label === "YourClassTwo") {
    background(100, 150, 255);   // cool
  } else {
    background(200);             // neutral / loading
  }
  image(video, 0, 0, 320, 240);
  fill(0);
  textSize(20);
  text(label, 10, 260);
}
```

```javascript
// Different text or message per class
function draw() {
  background(30);
  image(video, 0, 0, 320, 240);
  fill(255);
  textSize(22);
  if (label === "YourClassOne") {
    text("You're showing: Category One", 10, 270);
  } else if (label === "YourClassTwo") {
    text("You're showing: Category Two", 10, 270);
  }
}
```

```javascript
// Confidence + class → circle color and size
function draw() {
  background(20);
  image(video, 0, 0, 320, 240);
  let diameter = map(confidence, 0, 1, 20, 200);
  if (label === "YourClassOne") {
    fill(255, 80, 80);    // red for class one
  } else {
    fill(80, 200, 255);   // blue for class two
  }
  noStroke();
  circle(160, 340, diameter);
  fill(255);
  textSize(16);
  text(label + " — " + nf(confidence * 100, 2, 1) + "%", 10, 420);
}
```

> Replace `"YourClassOne"` and `"YourClassTwo"` with the exact class names you used in Teachable Machine — spelling and capitalization must match exactly.

**The pattern is always the same:** get a value from ml5 (a label, a confidence number, an x/y position) → use it as a variable in p5 (a color, a size, a position, a word). Start with one substitution, see what it does, then change something else.

| Model output | Creative use ideas |
|---|---|
| MobileNet label | Show a word that responds to the label; change background color per detection |
| Confidence score (0–1) | Control opacity, size, or speed of something on screen |
| Pose keypoint (x, y) | Draw at that position; measure distance between two points |
| Teachable Machine class | Show different text, image, or color for each class you defined |

---

## Step 4: Save and Share

1. Click **File > Save**
2. Click **File > Share** → copy the "Sketch" link
3. In your GitHub repo: click **"Add file"** → **"Create new file"** → name it `links.md` → paste the link → click **"Commit changes"**

---

## Troubleshooting

| Problem | Fix |
|---------|-----|
| "ml5 is not defined" | ml5 script tag is missing — make sure you replaced the full `index.html` contents in Step 1 |
| Camera permission denied | Refresh the page and click Allow when prompted |
| Model loads but nothing classifies | Check that `classifyVideo()` is called at the end of `setup()` |
| PoseNet runs but no keypoints appear | Check `poses.length > 0` before accessing `poses[0]`; give the model a second to warm up |
| Teachable Machine URL not working | Make sure you clicked **"Upload my model"** before copying the link — not just "Export" |
| Sketch runs slowly | Video + model is computationally heavy; close other browser tabs |
| Labels flash too fast | Make sure `setTimeout(classifyVideo, 500)` is at the end of `classifyVideo()` |
