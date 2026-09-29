# botany-itep
for education 
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<title>Mitosis Animation</title>

<style>
* {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
}

body {
    font-family: Arial, Helvetica, sans-serif;
    background: linear-gradient(135deg, #071426, #102a43, #06111f);
    color: white;
    min-height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
    padding: 30px;
}

.container {
    width: min(1100px, 100%);
    background: rgba(255,255,255,0.06);
    border: 1px solid rgba(255,255,255,0.15);
    border-radius: 25px;
    padding: 30px;
    backdrop-filter: blur(15px);
    box-shadow: 0 25px 80px rgba(0,0,0,0.45);
}

h1 {
    text-align: center;
    font-size: 38px;
    margin-bottom: 8px;
}

.subtitle {
    text-align: center;
    color: #a9c7df;
    margin-bottom: 25px;
}

/* ---------------- CELL ---------------- */

.cell-area {
    height: 500px;
    position: relative;
    display: flex;
    justify-content: center;
    align-items: center;
    overflow: hidden;
}

.cell {
    width: 390px;
    height: 390px;
    border-radius: 50%;
    border: 8px solid rgba(91, 210, 255, 0.75);
    background: radial-gradient(
        circle at 40% 35%,
        rgba(71,180,255,0.20),
        rgba(14,47,75,0.35)
    );
    box-shadow:
        0 0 30px rgba(61,191,255,0.35),
        inset 0 0 50px rgba(0,150,255,0.15);

    position: relative;
    transition: all 0.8s ease;
}

/* Nucleus */

.nucleus {
    position: absolute;
    width: 170px;
    height: 170px;
    border-radius: 50%;
    border: 5px solid rgba(198,123,255,0.8);
    background: rgba(109,53,155,0.25);
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
    transition: all 0.8s ease;
}

/* Nucleolus */

.nucleolus {
    width: 35px;
    height: 35px;
    border-radius: 50%;
    background: #e28aff;
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
    box-shadow: 0 0 20px #df72ff;
}

/* ---------------- CHROMOSOMES ---------------- */

.chromosome {
    position: absolute;
    width: 18px;
    height: 75px;
    left: 50%;
    top: 50%;
    transform-origin: center;
    transition: all 1s ease;
}

.chromosome::before,
.chromosome::after {
    content: "";
    position: absolute;
    width: 18px;
    height: 48px;
    border-radius: 12px;
    background: linear-gradient(#ff79c6, #b95cff);
    box-shadow: 0 0 12px rgba(255,100,220,0.7);
}

.chromosome::before {
    transform: rotate(45deg);
}

.chromosome::after {
    transform: rotate(-45deg);
}

/* ---------------- SPINDLE ---------------- */

.spindle {
    position: absolute;
    width: 100%;
    height: 100%;
    left: 0;
    top: 0;
    opacity: 0;
    transition: opacity 0.8s ease;
}

.pole {
    position: absolute;
    width: 18px;
    height: 18px;
    border-radius: 50%;
    background: #65e6ff;
    box-shadow: 0 0 20px #65e6ff;
    top: 50%;
    transform: translateY(-50%);
}

.pole.left {
    left: 25px;
}

.pole.right {
    right: 25px;
}

.fiber {
    position: absolute;
    height: 2px;
    background: rgba(111,220,255,0.7);
    top: 50%;
    left: 50%;
    transform-origin: left center;
    width: 150px;
}

/* ---------------- CYTOKINESIS ---------------- */

.cleavage {
    position: absolute;
    width: 30px;
    height: 120px;
    border-radius: 50%;
    border-left: 5px solid rgba(255,255,255,0.6);
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
    opacity: 0;
    transition: all 1s ease;
}

/* ---------------- INFO ---------------- */

.info {
    text-align: center;
    margin-top: 15px;
}

.stage {
    font-size: 30px;
    font-weight: bold;
    color: #66dcff;
    margin-bottom: 8px;
}

.description {
    color: #c6d9e8;
    line-height: 1.6;
    max-width: 750px;
    margin: auto;
}

/* ---------------- CONTROLS ---------------- */

.controls {
    display: flex;
    justify-content: center;
    align-items: center;
    gap: 12px;
    margin-top: 25px;
    flex-wrap: wrap;
}

button {
    border: none;
    padding: 12px 22px;
    border-radius: 12px;
    background: #1b6ca8;
    color: white;
    font-size: 15px;
    cursor: pointer;
    transition: 0.25s;
}

button:hover {
    transform: translateY(-2px);
    background: #2587c9;
}

.play {
    background: #9b4dca;
}

.play:hover {
    background: #b55be8;
}

/* ---------------- PROGRESS ---------------- */

.progress {
    display: flex;
    justify-content: center;
    gap: 8px;
    margin-top: 25px;
}

.dot {
    width: 12px;
    height: 12px;
    border-radius: 50%;
    background: #31536c;
    transition: 0.3s;
}

.dot.active {
    background: #66dcff;
    box-shadow: 0 0 12px #66dcff;
}

/* ==================================================
   STAGE 0 - INTERPHASE
================================================== */

.interphase .nucleus {
    opacity: 1;
}

.interphase .chromosome {
    opacity: 0;
}

.interphase .spindle {
    opacity: 0;
}

/* ==================================================
   STAGE 1 - PROPHASE
================================================== */

.prophase .nucleus {
    transform: translate(-50%, -50%) scale(0.65);
    opacity: 0.25;
}

.prophase .chromosome {
    opacity: 1;
}

/* ==================================================
   STAGE 2 - METAPHASE
================================================== */

.metaphase .nucleus {
    opacity: 0;
}

.metaphase .spindle {
    opacity: 1;
}

.metaphase .chromosome:nth-child(1) {
    transform: translate(-50%, -50%) translate(0px, -75px);
}

.metaphase .chromosome:nth-child(2) {
    transform: translate(-50%, -50%) translate(0px, -25px);
}

.metaphase .chromosome:nth-child(3) {
    transform: translate(-50%, -50%) translate(0px, 25px);
}

.metaphase .chromosome:nth-child(4) {
    transform: translate(-50%, -50%) translate(0px, 75px);
}

/* ==================================================
   STAGE 3 - ANAPHASE
================================================== */

.anaphase .nucleus {
    opacity: 0;
}

.anaphase .spindle {
    opacity: 1;
}

.anaphase .chromosome:nth-child(1) {
    transform: translate(-50%, -50%) translate(-100px, -75px);
}

.anaphase .chromosome:nth-child(2) {
    transform: translate(-50%, -50%) translate(-100px, -25px);
}

.anaphase .chromosome:nth-child(3) {
    transform: translate(-50%, -50%) translate(100px, 25px);
}

.anaphase .chromosome:nth-child(4) {
    transform: translate(-50%, -50%) translate(100px, 75px);
}

/* ==================================================
   STAGE 4 - TELOPHASE
================================================== */

.telophase .spindle {
    opacity: 0;
}

.telophase .chromosome:nth-child(1),
.telophase .chromosome:nth-child(2) {
    transform: translate(-50%, -50%) translate(-105px, 0) scale(0.65);
}

.telophase .chromosome:nth-child(3),
.telophase .chromosome:nth-child(4) {
    transform: translate(-50%, -50%) translate(105px, 0) scale(0.65);
}

.telophase .nucleus {
    opacity: 1;
    width: 105px;
    height: 105px;
}

/* ==================================================
   STAGE 5 - CYTOKINESIS
================================================== */

.cytokinesis .cell {
    width: 600px;
}

.cytokinesis .nucleus {
    opacity: 1;
    width: 105px;
    height: 105px;
    left: 25%;
}

.cytokinesis .chromosome:nth-child(1),
.cytokinesis .chromosome:nth-child(2) {
    transform: translate(-50%, -50%) translate(-105px, 0) scale(0.5);
}

.cytokinesis .chromosome:nth-child(3),
.cytokinesis .chromosome:nth-child(4) {
    transform: translate(-50%, -50%) translate(105px, 0) scale(0.5);
}

.cytokinesis .cleavage {
    opacity: 1;
    height: 300px;
}

/* ---------------- MOBILE ---------------- */

@media (max-width: 600px) {

    body {
        padding: 12px;
    }

    .container {
        padding: 18px;
    }

    h1 {
        font-size: 28px;
    }

    .cell-area {
        height: 390px;
    }

    .cell {
        width: 300px;
        height: 300px;
    }

    .cytokinesis .cell {
        width: 340px;
    }

    .nucleus {
        width: 130px;
        height: 130px;
    }

    .description {
        font-size: 14px;
    }
}
</style>
</head>

<body>

<div class="container">

    <h1>🧬 Mitosis</h1>

    <p class="subtitle">
        Interactive animation of cell division
    </p>

    <div class="cell-area">

        <div id="cell" class="cell interphase">

            <!-- Nucleus -->
            <div class="nucleus">
                <div class="nucleolus"></div>
            </div>

            <!-- Chromosomes -->
            <div class="chromosome"></div>
            <div class="chromosome"></div>
            <div class="chromosome"></div>
            <div class="chromosome"></div>

            <!-- Spindle -->
            <div class="spindle">

                <div class="pole left"></div>
                <div class="pole right"></div>

                <div class="fiber"
                     style="transform: rotate(0deg);">
                </div>

                <div class="fiber"
                     style="transform: rotate(45deg);">
                </div>

                <div class="fiber"
                     style="transform: rotate(-45deg);">
                </div>

                <div class="fiber"
                     style="transform: rotate(180deg);">
                </div>

            </div>

            <!-- Cleavage furrow -->
            <div class="cleavage"></div>

        </div>

    </div>

    <!-- Information -->

    <div class="info">

        <div id="stage" class="stage">
            Interphase
        </div>

        <p id="description" class="description">
            The cell grows and prepares for division. DNA is replicated,
            producing two identical copies of each chromosome.
        </p>

    </div>

    <!-- Progress -->

    <div class="progress">

        <div class="dot active"></div>
        <div class="dot"></div>
        <div class="dot"></div>
        <div class="dot"></div>
        <div class="dot"></div>
        <div class="dot"></div>

    </div>

    <!-- Controls -->

    <div class="controls">

        <button onclick="previousStage()">
            ← Previous
        </button>

        <button class="play" onclick="togglePlay()" id="playButton">
            ▶ Play
        </button>

        <button onclick="nextStage()">
            Next →
        </button>

    </div>

</div>


<script>

/* ==================================================
   MITOSIS DATA
================================================== */

const stages = [

    {
        name: "Interphase",

        description:
        "The cell grows and prepares for division. DNA is replicated, producing two identical copies of each chromosome.",

        className: "interphase"
    },

    {
        name: "Prophase",

        description:
        "Chromatin condenses into visible chromosomes. The nuclear envelope begins to break down and the mitotic spindle begins to form.",

        className: "prophase"
    },

    {
        name: "Metaphase",

        description:
        "Chromosomes attach to spindle fibres and align along the equatorial plane of the cell.",

        className: "metaphase"
    },

    {
        name: "Anaphase",

        description:
        "Sister chromatids separate at the centromere and move toward opposite poles of the cell.",

        className: "anaphase"
    },

    {
        name: "Telophase",

        description:
        "Chromosomes reach opposite poles and begin to decondense. New nuclear envelopes form around each chromosome set.",

        className: "telophase"
    },

    {
        name: "Cytokinesis",

        description:
        "The cytoplasm divides, producing two genetically similar daughter cells.",

        className: "cytokinesis"
    }

];


/* ==================================================
   VARIABLES
================================================== */

let currentStage = 0;

let playing = false;

let timer;


/* ==================================================
   ELEMENTS
================================================== */

const cell =
document.getElementById("cell");

const stageTitle =
document.getElementById("stage");

const description =
document.getElementById("description");

const dots =
document.querySelectorAll(".dot");

const playButton =
document.getElementById("playButton");


/* ==================================================
   SHOW STAGE
================================================== */

function showStage(index) {

    currentStage = index;

    const stage =
    stages[currentStage];

    /* Remove all stage classes */

    cell.classList.remove(
        "interphase",
        "prophase",
        "metaphase",
        "anaphase",
        "telophase",
        "cytokinesis"
    );

    /* Add current stage */

    cell.classList.add(stage.className);

    /* Update text */

    stageTitle.textContent =
    stage.name;

    description.textContent =
    stage.description;

    /* Update dots */

    dots.forEach((dot, i) => {

        dot.classList.toggle(
            "active",
            i === currentStage
        );

    });

}


/* ==================================================
   NEXT
================================================== */

function nextStage() {

    currentStage++;

    if (currentStage >= stages.length) {

        currentStage = 0;

    }

    showStage(currentStage);

}


/* ==================================================
   PREVIOUS
================================================== */

function previousStage() {

    currentStage--;

    if (currentStage < 0) {

        currentStage =
        stages.length - 1;

    }

    showStage(currentStage);

}


/* ==================================================
   PLAY / PAUSE
================================================== */

function togglePlay() {

    if (playing) {

        stopAnimation();

    } else {

        startAnimation();

    }

}


/* ==================================================
   START
================================================== */

function startAnimation() {

    playing = true;

    playButton.textContent =
    "⏸ Pause";

    timer = setInterval(() => {

        nextStage();

    }, 3000);

}


/* ==================================================
   STOP
================================================== */

function stopAnimation() {

    playing = false;

    playButton.textContent =
    "▶ Play";

    clearInterval(timer);

}


/* ==================================================
   INITIALIZE
================================================== */

showStage(0);

</script>

</body>
</html>
