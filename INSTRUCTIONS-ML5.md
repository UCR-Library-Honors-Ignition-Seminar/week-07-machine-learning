# Step-by-Step: Building an ml5.js + p5.js Sketch

**Time needed:** 45–60 minutes | **Requires:** webcam | **No downloads required**

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

Pick **one** of the three options below.

### Option A — MobileNet or Option B — PoseNet

Open [README.md](README.md) and go to **Session 2 → Step 2**. You will find:

- **Option A — MobileNet:** a pretrained model that labels everyday objects from your webcam
- **Option B — PoseNet:** a model that tracks 17 body keypoints (nose, wrists, shoulders, hips) in real time

Copy the full code block for your chosen option, then in `sketch.js`: **select all** (Cmd+A / Ctrl+A) and paste to replace everything. Click Play (▶) and allow camera access when prompted.

> If you are not sure which to pick: start with **Option A**. It is easier to get running, and the confidence score gives you something interesting to work with right away.

---

### Option C — Your Teachable Machine Model

Use this option if you want to use the model you trained in Session 1, with the exact class names you defined.

**First, export your model from Teachable Machine:**
1. In Teachable Machine, click **"Export Model"**
2. Choose **"Tensorflow.js"** → **"Upload (shareable link)"**
3. Click **"Upload my model"** — you get a URL like `https://teachablemachine.withgoogle.com/models/XXXXXXX/`
4. Copy that URL

**Then paste this code into `sketch.js`** (select all, replace everything). Replace `YOUR_MODEL_URL` with the URL you copied:

```javascript
// ML5 + p5.js starter: Teachable Machine classifier
// Uses the model you trained — your class names appear as labels.
// Replace YOUR_MODEL_URL with the link from Teachable Machine > Export Model.

let classifier, video;
let label = "loading model...";
let confidence = 0;

const MODEL_URL = "YOUR_MODEL_URL";

async function setup() {
  createCanvas(640, 480);
  video = createCapture(VIDEO);
  video.hide();
  // wait for your model to load — takes a few seconds
  classifier = await ml5.imageClassifier(MODEL_URL, video);
  console.log("Model ready!");
  classifyVideo();
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

Click Play. Allow camera access. Point your webcam at one of the categories you trained on — you should see your own class names appearing at the bottom.

---

## Step 3: Make the Output Do Something

### If you chose Option A (MobileNet) or Option B (PoseNet)

Open [README.md](README.md) and go to **Session 2 → Step 3**. It has ready-to-paste modifications for each option — replace just the `draw()` function with one of those examples, or use them as a starting point to make your own changes.

### If you chose Option C (Teachable Machine)

Because you named the classes yourself, you can use `label` directly to branch on your own category names. Replace the `draw()` function with one of these:

**Different background color per class:**
```javascript
function draw() {
  if (label === "YourClassOne") {
    background(255, 200, 100);   // warm
  } else if (label === "YourClassTwo") {
    background(100, 150, 255);   // cool
  } else {
    background(200);             // neutral / still loading
  }
  image(video, 0, 0, 320, 240);
  fill(0);
  textSize(20);
  text(label, 10, 260);
}
```

**Different text or message per class:**
```javascript
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

**Confidence + class → circle color and size:**
```javascript
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

> Replace `"YourClassOne"` and `"YourClassTwo"` with the exact names you used in Teachable Machine — spelling and capitalization must match exactly.

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
| No keypoints or detections appear | Check that your `if` condition runs before accessing the result — e.g. `if (poses.length > 0)`; give the model a few seconds to warm up |
| Teachable Machine URL not working | Make sure you clicked **"Upload my model"** before copying the link — not just "Export" |
| Sketch runs slowly | Video + model is computationally heavy; close other browser tabs |
| Labels flash too fast | Make sure `setTimeout(classifyVideo, 500)` is at the end of `classifyVideo()` |
