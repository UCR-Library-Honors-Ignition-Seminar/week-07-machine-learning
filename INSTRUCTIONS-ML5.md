# Step-by-Step: Building an ml5.js + p5.js Sketch

**Time needed:** 45–60 minutes | **Requires:** webcam | **No downloads required**

---

## What is ml5.js?

ml5.js is a JavaScript library that makes machine learning models accessible from a browser sketch. It sits on top of p5.js — you use both together. ml5 loads the model; p5 draws the output.

You do not need to understand how the model works internally. You need to know: what does it take as input, and what does it give back?

---

## Step 1: Open the p5.js Editor

Go to [editor.p5js.org](https://editor.p5js.org).

You need to load ml5.js in addition to p5.js. In the editor:
1. Click the **"Sketch Files"** arrow (top left)
2. Open `index.html`
3. Add this line in the `<head>` section, above the p5 script tag:

```html
<script src="https://unpkg.com/ml5@latest/dist/ml5.min.js"></script>
```

Your `index.html` head should look like:
```html
<head>
  <script src="https://unpkg.com/ml5@latest/dist/ml5.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/p5.js/1.9.0/p5.min.js"></script>
</head>
```

---

## Step 2: Allow Camera Access

ml5 needs your webcam. When the sketch runs, your browser will ask for camera permission — click Allow.

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

```javascript
let classifier, video;
let label = "loading model...";
let confidence = 0;

function setup() {
  createCanvas(640, 480);
  video = createCapture(VIDEO);
  video.hide();

  // load MobileNet and start classifying when ready
  classifier = ml5.imageClassifier("MobileNet", video, modelReady);
}

function modelReady() {
  console.log("Model ready!");
  classifier.classify(gotResult); // start the classification loop
}

function gotResult(results) {
  label = results[0].label;
  confidence = nf(results[0].confidence * 100, 2, 1); // format as percentage
  classifier.classify(gotResult); // classify again immediately
}

function draw() {
  image(video, 0, 0, width, height);

  // draw label at bottom
  fill(0, 0, 0, 150);
  noStroke();
  rect(0, height - 60, width, 60);
  fill(255);
  textSize(20);
  textAlign(LEFT, CENTER);
  text(label + " (" + confidence + "%)", 10, height - 30);
}
```

### Option B: Your Teachable Machine model

1. In Teachable Machine, click **"Export Model"**
2. Choose **"Tensorflow.js"** → **"Upload (shareable link)"**
3. Click **"Upload my model"** — you get a URL like `https://teachablemachine.withgoogle.com/models/XXXXXXX/`
4. Replace `"MobileNet"` in the code above with that URL:

```javascript
classifier = ml5.imageClassifier("https://teachablemachine.withgoogle.com/models/XXXXXXX/", video, modelReady);
```

Everything else stays the same.

### Option C: PoseNet (detects 17 body keypoints)

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

    // draw a circle at the nose
    fill(255, 0, 0);
    noStroke();
    circle(pose.nose.x, pose.nose.y, 30);

    // available keypoints:
    // pose.nose, pose.leftEye, pose.rightEye
    // pose.leftShoulder, pose.rightShoulder
    // pose.leftWrist, pose.rightWrist
    // pose.leftHip, pose.rightHip
    // pose.leftAnkle, pose.rightAnkle
  }
}
```

---

## Step 4: Make the Output Do Something

Displaying a label as text is a starting point. The goal is to use the model's output as a creative material.

**Using the label to change visuals:**
```javascript
function draw() {
  // change background color based on what the model sees
  if (label.includes("person")) {
    background(50, 50, 200); // blue when a person is detected
  } else {
    background(200, 50, 50); // red otherwise
  }
  image(video, 0, 0, 320, 240); // smaller video
}
```

**Using confidence score as a variable:**
```javascript
function draw() {
  background(0);
  image(video, 0, 0, width, height);

  // confidence ranges from 0 to 1 — map it to circle size
  let size = map(results[0].confidence, 0, 1, 10, 300);
  fill(255, 200, 0, 150);
  noStroke();
  circle(width / 2, height / 2, size);
}
```

**Using pose keypoints to draw:**
```javascript
function draw() {
  background(0);
  if (poses.length > 0) {
    let wrist = poses[0].pose.rightWrist;
    // draw a trail of circles following the right wrist
    fill(255, 100, 0, 100);
    noStroke();
    circle(wrist.x, wrist.y, 40);
  }
}
```

---

## Step 5: Save and Share

1. Click **File > Save** (sign in to save to your account)
2. Click **File > Share** → copy the "Sketch" link
3. Add the link to `links.md` in your GitHub repo

Or: **File > Download** → upload the folder contents to your repo.

---

## Troubleshooting

| Problem | Fix |
|---------|-----|
| "ml5 is not defined" | ml5 script tag is missing or in wrong order in `index.html` |
| Camera permission denied | Refresh the page and click Allow when prompted |
| Model loads but nothing classifies | Make sure `classifier.classify(gotResult)` is called inside `modelReady()` |
| PoseNet runs but no keypoints appear | Check `poses.length > 0` before accessing `poses[0]`; give the model a second to warm up |
| Teachable Machine URL not working | Make sure you clicked "Upload" in Teachable Machine before copying the link — not just "Export" |
| Sketch runs slowly | The video + model is computationally heavy; close other browser tabs |
