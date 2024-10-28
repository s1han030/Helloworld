let gridSize = 6;
let rectSize = 116.5;
let emojis = ["🐶", "🐱", "🐭", "🐹", "🐰", "🦊", "🐻", "🐼", "🐨", "🐵", 
              "🐸", "🐷", "🐔", "🐘", "🐧", "🦄", "🦉", "🐍"];
let grid = [];
let revealed = [];
let clicks = 0;
let firstIndex, secondIndex;
let gameStart = false;
let startTime;
let bestTime = Infinity;
let timer;

function setup() {
    createCanvas(820, 700);
    for (let i = 0; i < gridSize; i++) {
        grid[i] = [];
        revealed[i] = [];
        for (let j = 0; j < gridSize; j++) {
            grid[i][j] = null;
            revealed[i][j] = false;
        }
    }
    setUpGame();
    // createButtons();
}

function draw() {
    background(255);
    drawGrid();
    if (gameStart) {
        let elapsedTime = (millis() - startTime) / 1000;
        fill(0);
        textSize(12);
        textAlign(RIGHT, CENTER); // Set text alignment to right
        text('Time: ' + floor(elapsedTime) + 's', width - 22, height - 40); // Lower right corner

        // Update the best time
        if (elapsedTime < bestTime) {
            bestTime = elapsedTime;
        }
    }
    
    // Display best time
    fill(0);
    textSize(12);
    textAlign(RIGHT, CENTER); // Set text alignment to right
    text('Best Time: ' + (bestTime === Infinity ? 'N/A' : floor(bestTime)) + 's', width - 22, height - 60); // Lower right corner
}


function drawGrid() {
    for (let i = 0; i < gridSize; i++) {
        for (let j = 0; j < gridSize; j++) {
            stroke(150);
            fill(revealed[i][j] ? 200 : 255);
            rect(j * rectSize, i * rectSize, rectSize, rectSize, 20);
            if (revealed[i][j]) {
                fill(0);
                textSize(40);
                textAlign(CENTER, CENTER);
                text(grid[i][j], j * rectSize + rectSize / 2, i * rectSize + rectSize / 2);
            }
        }
    }
}

function mousePressed() {
    if (!gameStart) {
        startGame();
    } else if (clicks < 2) {
        let row = floor(mouseY / rectSize);
        let col = floor(mouseX / rectSize);
        if (row < gridSize && col < gridSize && !revealed[row][col]) {
            revealed[row][col] = true;
            clicks++;

            if (clicks === 1) {
                firstIndex = { row, col };
            } else if (clicks === 2) {
                secondIndex = { row, col };
                checkMatch();
            }
        }
    }
}

function setUpGame() {
    let emojiList = [...emojis, ...emojis]; // Each emoji appears twice
    emojiList = shuffleArray(emojiList); // Shuffle emojis
    for (let i = 0; i < gridSize; i++) {
        for (let j = 0; j < gridSize; j++) {
            grid[i][j] = emojiList.pop();
            revealed[i][j] = false; // Initialize all squares as hidden
        }
    }
}

function shuffleArray(array) { 
    for (let i = array.length - 1; i > 0; i--) {
        const j = floor(random(i + 1));
        [array[i], array[j]] = [array[j], array[i]];
    }
    return array;
}

function startGame() {
    startTime = millis();
    gameStart = true;
    setUpGame();
    clicks = 0;
}

function checkMatch() {
    setTimeout(() => {
        if (grid[firstIndex.row][firstIndex.col] !== grid[secondIndex.row][secondIndex.col]) {
            // If they don't match, hide both squares
            revealed[firstIndex.row][firstIndex.col] = false;
            revealed[secondIndex.row][secondIndex.col] = false;
        } else {
            // If it matches, check that the game is over
            checkGameEnd();
        }
        clicks = 0; // Allow next click
    }, 1000);
}

function checkGameEnd() {
    let allRevealed = revealed.every(row => row.every(cell => cell));
    if (allRevealed) {
        let elapsedTime = floor((millis() - startTime) / 1000);
        alert('Game Over! Time: ' + elapsedTime + ' seconds');
        
        // Update the best time
        if (elapsedTime < bestTime) {
            bestTime = elapsedTime;
        }
        // Stop timing and reset click count at the end of the game
        gameStart = false; // Stop the game state
        clicks = 0; // Reset clicks
    }
}
