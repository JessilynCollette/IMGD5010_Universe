# Inspiration
I was inspired to expand on the use of bubbles from my last assignment. I wanted to continue with that to tell a story. I used fast pacing to build suspense and lead to the climax of the animation where it then slows down and fades away. I wanted this piece to have an observable beat without any sound being added. I felt like it tied into the explosion aspect, as your surroundings go silent and a bright light appears when an exlposion happens.  

## Structure
This piece starts off with a single circle in the center of the screen that grows and changes colors. This circle is surrounded by pulsing sound waves as it get larger. When the circle reaches its maximum size it exlpodes into varying, smaller sized bubbles that shrink and eventually pop. Before the explosion happens the entire screen is black, but after it turns white. The animation then loops to repeat the process until the code is stopped. 

## Code
```p5.js
let bubbles = [];
let isGrowing = true;
let circleSize = 1; // for main circle
let centerX, centerY;
let hasExploded = false;
let soundWave = 0;

// Color palette
let colors = [
  [34, 87, 122], 
  [56, 163, 165],   
  [87, 204, 153], 
  [108, 90, 73]
];

let colorSpeed = 0.2; // color change speed

function setup() {
  createCanvas(600, 600);
  centerX = width / 2;
  centerY = height / 2;
  frameRate(30); // lower fr made more smooth
}

// Create bubbles at random positions
function createBubbles() {
  for (let i = 0; i < 150; i++) {
    let angle = random(TWO_PI); 
    let x = centerX + circleSize * cos(angle); 
    let y = centerY + circleSize * sin(angle);
    let direction = createVector(cos(angle), sin(angle));
    bubbles.push(new Bubble(x, y, random(10, 25), direction, random(1, 2))); // Add new bubble
  }
}

function draw() {
  if (!hasExploded) {
    background(0); // Black background before explosion
  } else {
    background(255); // White background after explosion
  }

  // Make the main circle's color shift
  let colorShift = (sin(frameCount * colorSpeed) + 1) / 2; // Sin wave for smooth color transition
  let currentColor = color(colors[floor(colorShift * (colors.length - 1))]);
  let nextColor = color(colors[(floor(colorShift * (colors.length - 1)) + 1) % colors.length]);
  let blendedColor = lerpColor(currentColor, nextColor, colorShift); // Blend the two colors

  // Grow main circle and change its color
  if (isGrowing) {
    fill(blendedColor);
    noStroke();
    ellipse(centerX, centerY, circleSize * 2);
    circleSize++;

    // Draw pulsating sound waves
    soundWave = sin(frameCount * 0.2) * 50; // Pulsing effect
    for (let i = 0; i < 5; i++) {
      stroke(255, 100);
      noFill();
      ellipse(centerX, centerY, circleSize * 2 + soundWave * i);
    }
    // if bubble reaches 150, explode
    if (circleSize >= 150) {
      isGrowing = false;
      createBubbles(); // exploding bubble particles 
      hasExploded = true;
    }
  }
  
  // Update and display bubbles
  for (let i = bubbles.length - 1; i >= 0; i--) {
    let bubble = bubbles[i]; // bubble object into bubble array
    bubble.update(); // Update bubble position and size
    bubble.display(); // Draw the bubble
    if (bubble.size <= 1) bubbles.splice(i, 1); // Remove bubble if too small
  }
  
  // Reset animation after explosion
  if (bubbles.length === 0 && !isGrowing && hasExploded) {
    isGrowing = true;
    circleSize = 1;
    hasExploded = false;
  }
}

class Bubble {
  constructor(x, y, size, direction, speed) { // create object of class bubble
    this.x = x; 
    this.y = y;
    this.size = size;
    this.direction = direction;
    this.speed = speed;
    this.color = random(colors);
  }
  
  // Update bubble position and size over time
  update() {
    this.x += this.direction.x * this.speed; 
    this.y += this.direction.y * this.speed;
    this.size *= 0.97; // Shrink bubble over time "pop"
  }
  
  display() {
    stroke(this.color);
    fill(this.color[0], this.color[1], this.color[2], 150);
    ellipse(this.x, this.y, this.size * 2);
  }
}
```

## Link
https://editor.p5js.org/JessilynCollette/sketches/yTO6pPjE7

