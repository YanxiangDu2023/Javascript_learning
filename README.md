# Javascript_learning

### Snake Game

This code implements a simple Snake game with the following features:

1. **Basic Game Functionality**:
   - Players can control the snake's movement using the arrow keys.
   - The snake moves automatically, and players must control the direction to avoid hitting walls or biting its own body.

2. **Food Generation and Scoring**:
   - Food (eggs) is randomly generated in the game.
   - When the snake eats the food, its body grows longer, and the score increases.

3. **Collision Detection**:
   - The game ends if the snake's head collides with the walls or its own body.

4. **Timer**:
   - The game includes a timer that displays the duration of the game in minutes and seconds.

5. **Auto Play Mode** (experimental feature):
   - There is an option to enable auto play mode, where the snake automatically searches for food.

6. **Leveling Up**:
   - The game's speed increases every time the snake eats a certain number of food items, increasing the difficulty.
   - When a specific score is reached, a "Congratulations" message is displayed, indicating the player has won the game.

### Main Code Structure

#### HTML
- Defines the basic layout of the game, including the scoreboard, timer, and game area.

#### CSS
- The stylesheet (`tss.css`) is used to define the visual appearance of the game, such as the snake and food styles.

#### JavaScript
- Controls the game logic, including snake movement, collision detection, food generation, and score tracking.

```html
<script>
// Initialize the snake's attributes
var tss_arr = [
    {left: 0, top: 0},
    {left: 0, top: 0},
    {left: 0, top: 0},
    {left: 0, top: 0},
    {left: 0, top: 0},
    {left: 0, top: 0}
];

// Snake head position
var tss_head = {left: 0, top: 0};

// Snake movement direction
var tss_turn = 39; // Initial direction is right

// Control data for snake movement
var tss_control = {
    37: {left: -20, top: 0, class: "left"},  // Left
    38: {left: 0, top: -20, class: "top"},   // Up
    39: {left: 20, top: 0, class: "right"},  // Right
    40: {left: 0, top: 20, class: "bottom"}  // Down
};

// Score tracking
var score = 0;
var score_doms = document.querySelector(".score-text");

// Timer
var s = 0;
var sec_doms = document.querySelector(".sec");
var min_doms = document.querySelector(".min");

// Snake movement function
function snakemove() {
    // Update snake head position
    tss_head.left += tss_control[tss_turn].left;
    tss_head.top += tss_control[tss_turn].top;

    // Collision detection
    collisionDetection();
    
    // Update each segment of the snake's position
    for (var i = tss_arr.length - 1; i > 0; i--) {
        tss_arr[i] = {...tss_arr[i-1]};
    }
    tss_arr[0] = {...tss_head};

    // Update DOM
    updateDOM();
}

// Collision detection function
function collisionDetection() {
    // Check if the snake bites itself or hits a wall
    if (tss_head.left < 0 || tss_head.left > 800 || tss_head.top < 0 || tss_head.top > 600) {
        clearInterval(t);
        clearInterval(tm);
        alert("Game Over");
    }
    for (var i = 1; i < tss_arr.length; i++) {
        if (tss_head.left === tss_arr[i].left && tss_head.top === tss_arr[i].top) {
            clearInterval(t);
            clearInterval(tm);
            alert("Game Over");
        }
    }
}

// Timer function
function timeMeter() {
    s++;
    var min = parseInt(s / 60);
    var sec = s % 60;
    if (min < 10) min = "0" + min;
    if (sec < 10) sec = "0" + sec;
    sec_doms.innerHTML = sec;
    min_doms.innerHTML = min;
}

var t = setInterval(snakemove, 300);
var tm = setInterval(timeMeter, 1000);

// Keyboard event to control snake's direction
document.onkeydown = function(e) {
    if (e.keyCode >= 37 && e.keyCode <= 40 && Math.abs(tss_turn - e.keyCode) !== 2) {
        tss_turn = e.keyCode;
    }
}

// Create food
function createEgg() {
    // Randomly generate food position
    var egg = {
        left: parseInt(Math.random() * 40) * 20,
        top: parseInt(Math.random() * 30) * 20,
        class: "tss-egg",
        dom: document.createElement("span")
    };
    egg.dom.style.left = egg.left + "px";
    egg.dom.style.top = egg.top + "px";
    egg.dom.classList.add(egg.class);
    document.querySelector(".tss-wrap").appendChild(egg.dom);
    return egg;
}

var egg = createEgg();

// Food detection
function eggDetection() {
    if (tss_head.left === egg.left && tss_head.top === egg.top) {
        score++;
        score_doms.innerHTML = score;
        egg.dom.remove();
        egg = createEgg();
        tss_arr.unshift({left: tss_arr[0].left, top: tss_arr[0].top});
    }
}

// Update DOM
function updateDOM() {
    var tss_doms = document.querySelectorAll(".tss-wrap div");
    for (var i = 0; i < tss_doms.length; i++) {
        tss_doms[i].style.left = tss_arr[i].left + "px";
        tss_doms[i].style.top = tss_arr[i].top + "px";
    }
    eggDetection();
}
</script>
```

### Instructions
1. Open the `index.html` file and run the game in a web browser.
2. Use the arrow keys (up, down, left, right) to control the snake's movement.
3. When the snake eats the food, it will grow longer and the score will increase.
4. The game records the time and score. If the snake hits the wall or its own body, the game will end.

### Additional Information
- You can adjust the game speed by modifying the interval timer (e.g., `setInterval(snakemove, 300)`).
- The auto-play mode and other advanced features are under development.
