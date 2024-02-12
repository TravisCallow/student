---
toc: false
comments: false
layout: post
title: Week 7 Final Game
description: Cool Game
type: plans
courses: { compsci: {week: 7} }
---





<style>
    #canvas {
        margin: 0;
        border: 1px solid white;
        background: black;
    }
</style>
<canvas id="canvas"></canvas>
<script>
    // Create empty canvas
    let canvas = document.getElementById('canvas');
    let c = canvas.getContext('2d');
    // Set the canvas dimensions
    canvas.width = 650;
    canvas.height = 400;
    canvas.style.webkitFilter = "blur(0.25px)";
    // Set gravity value
    let gravity = 1.5;
    // Facing Value | true = right, false = left
    let facing = false;
    // Game start
    let gamestarted = false;
    // Score
    let score = 0;
    // Spawn Location
    let pSpawnX = 100;
    let pSpawnY = 200;
    // Health
    let lives = 3;
    // Enemy Speed
    let enemySpeed = 0.25;
    let enemyCap = 3;
    // Define the Player class
    class Player {
        constructor() {
            // Initial position and velocity of the player
            this.position = {
                x: pSpawnX,
                y: pSpawnY
            };
            this.velocity = {
                x: 0,
                y: 0
            };
            // Dimensions of the player
            this.width = 30;
            this.height = 30;
        }
        // Method to draw the player on the canvas
        draw() {
            c.fillStyle = 'yellow';
            c.fillRect(this.position.x, this.position.y, this.width, this.height);
        }
        // Method to update the player position and velocity
        update() {
            this.draw();
            this.position.y += this.velocity.y;
            this.position.x += this.velocity.x;
            // Apply gravity if player is not at the bottom
            if (this.position.y + this.height + this.velocity.y <= canvas.height)
                this.velocity.y += gravity;
            else
                this.velocity.y = 0;
        }
    }
    class Enemy {
        constructor() {
            // Initial position and velocity of the enemy
            this.position = {
                x: 500,
                y: 200
            };
            this.velocity = {
                x: 0,
                y: 0
            };
            // Dimensions of the enemy
            this.width = 30;
            this.height = 30;
        }
        // Method to draw the enemy on the canvas
        draw() {
            c.fillStyle = 'red';
            c.fillRect(this.position.x, this.position.y, this.width, this.height);
        }
        // Method to update the enemy position and velocity
        update() {
            this.draw();
            this.position.y += this.velocity.y;
            this.position.x += this.velocity.x;
            // Apply gravity if enemy is not at the bottom
            if (this.position.y + this.height + this.velocity.y <= canvas.height - 40)  /////////////////CHANGE BACK TO this.position.y + this.height + this.velocity.y <= canvas.height ONCE GAME DONE
                this.velocity.y += gravity;
            else
                this.velocity.y = 0;
        }
    }
    //Make Sword
    class Sword {
        constructor(){
            this.position = {
                x: 100,
                y: 200
            };
            // Dimensions of the sword
            this.width = 5;
            this.height = 35;
        }
         // Method to draw the player on the canvas
        draw() {
            c.fillStyle = 'purple';
            c.fillRect(this.position.x, this.position.y, this.width, this.height);
        }
        // Method to update the player position and velocity
        update() {
            this.draw();
        }
    }
    //Text
    var ctx = canvas.getContext("2d");
    // Set the font style
    ctx.font = "20px monospace"; // You can customize the font size and type
    // Set the text color
    ctx.fillStyle = "black"; // You can customize the text color
    // Define the Platform class
    class Platform {
        constructor() {
            // Initial position of the platform
            this.position = {
                x: 0,
                y: 360
            }
            //this.image = image;
            this.width = 50000;
            this.height = 40;
        }
        // Method to draw the platform on the canvas
        draw() {
            c.fillStyle = 'green';
            c.fillRect(this.position.x, this.position.y, this.width, this.height);
        }
        update() {
            this.draw()
        }
    }
    //hearts
    class Heart {
        constructor() {
            // Initial position of the platform
            this.position = {
                x: 0,
                y: 0
            }
            //this.image = image;
            this.width = 25;
            this.height = 25;
        }
        // Method to draw the platform on the canvas
        draw() {
            c.fillStyle = 'red';
            c.fillRect(this.position.x, this.position.y, this.width, this.height);
        }
        update() {
            this.draw()
        }
    }
    // Define the Tube class
    class Tube {
        constructor(image) {
            // Initial position of the tube
            this.position = {
                x: 500,
                y: 180
            }
            this.image = image;
            this.width = 100;
            this.height = 120;
        }
        // Method to draw the tube on the canvas
        draw() {
            c.drawImage(this.image, this.position.x, this.position.y, this.width, this.height);
        }
    }
    // Define the BlockObject class
    class BlockObject {
        constructor(image) {
            // Initial position of the block object
            this.position = {
                x: 200,
                y: 100
            };
            this.image = image;
            this.width = 158;
            this.height = 79;
        }
        // Method to draw the block object on the canvas
        draw() {
            c.drawImage(this.image, this.position.x, this.position.y);
        }
    }
    //--
    // NEW CODE - CREATE GENERICOBJECT CLASS FOR THE BACKGROUND IMAGES
    //--
    class GenericObject {
        constructor({ x, y, image }) {
            this.position = {
                x,
                y
            };
            this.image = image;
            this.width = 760;
            this.height = 82;
        }
        // Method to draw the generic object on the canvas
        draw() {
            c.drawImage(this.image, this.position.x, this.position.y);
        }
    }
        // Load image sources
    let image = new Image();
    let imageTube = new Image();
    let imageBlock = new Image();
    //--
    // NEW CODE - ADD IMAGES FOR BACKGROUND
    //--
    let imageBackground = new Image();
    let imageHills = new Image();
    image.src = 'https://samayass.github.io/samayaCSA/images/platform.png';
    imageTube.src = 'https://samayass.github.io/samayaCSA/images/tube.png';
    imageBlock.src = 'https://samayass.github.io/samayaCSA/images/box.png';
    //--
    // NEW CODE - IMAGE URLS FOR BACKGROUND IMAGES
    //--
    imageBackground.src = 'https://samayass.github.io/samayaCSA/images/background.png';
    imageHills.src = '{{site.baseurl}}/images/Sonic_hedgehog_background.png'
    // Create instances of platform, tube, block object, and generic objects
    let platform = new Platform(image);
    let tube = new Tube(imageTube);
    let blockObject = new BlockObject(imageBlock);
    let sword = new Sword();
    //--
    // NEW CODE - CREATE ARRAY FOR GENERIC OBJECTS THEN ADD THE HILLS AND BACKGROUND
    //--
        let genericObjects = [
            new GenericObject({
                x:0, y:0, image: imageBackground
            }),
            new GenericObject({
                x:0, y:-150, image: imageHills
            }), 
            new GenericObject({
                x:650, y:-150, image: imageHills
            }),
            new GenericObject({
                x:1300, y:-150, image: imageHills
            })  
        ];
     // Track current background index
let currentBackgroundIndex = 0; // Start with the first background
// Function to update background based on player position
function updateBackground(playerX) {
    const numBackgrounds = genericObjects.length;
    if (playerX < 0) {
        // Move to the previous background
        currentBackgroundIndex--;
        if (currentBackgroundIndex < 0) {
            currentBackgroundIndex = numBackgrounds - 1; // Wrap around to the last background
        }
        genericObjects[0].image = 'background' + (currentBackgroundIndex + 1) + '.jpg'; // Update background image
    } else if (playerX > 650) { // Adjusted width for the game area
    console.log('detect at edge')
        currentBackgroundIndex++;
        if (currentBackgroundIndex >= numBackgrounds) {
            currentBackgroundIndex = 0; // Wrap around to the first background
        }
        genericObjects[0].image = '{{site.baseurl}}/images/Sonic_hedgehog_background.png';
    }
}
// Instantiate other game objects
player = new Player();
enemy = new Enemy();
let enemyHealth = 3;
sword = new Sword();
heart1 = new Heart();
heart1.position.x = 500;
heart1.position.y = 40;
heart2 = new Heart();
heart2.position.x = 540;
heart2.position.y = 40;
heart3 = new Heart();
heart3.position.x = 580;
heart3.position.y = 40;
    // Define keys and their states
    let keys = {
        right: {
            pressed: false
        },
        left: {
            pressed: false
        }
    }; 
    // Define the boundaries of the game area
const minX = 0;  // Minimum X coordinate
const maxX = canvas.width;  // Maximum X coordinate based on canvas width
const minY = 0;  // Minimum Y coordinate
const maxY = canvas.height;  // Maximum Y coordinate based on canvas height
// Function to check if the player is within the game boundaries
function isPlayerWithinBoundaries(playerX, playerY) {
    return playerX >= minX && playerX <= maxX && playerY >= minY && playerY <= maxY;
}
  // Animation loop
function animate() {
    requestAnimationFrame(animate);
    c.clearRect(0, 0, canvas.width, canvas.height);
    // Update background based on player position
    updateBackground(player.position.x);
    // Check if player is within the game boundaries
    if (!isPlayerWithinBoundaries(player.position.x, player.position.y)) {
        // If player is outside the boundaries, prevent further movement
        // For example, you could reset the player's position to stay within boundaries
        player.position.x = Math.max(minX, Math.min(maxX, player.position.x));
        player.position.y = Math.max(minY, Math.min(maxY, player.position.y));
    }
    // Render game objects and handle game logic
    if (gamestarted == false) {
        // Handle game initialization or intro screen rendering
        c.fillStyle = 'skyblue';
        c.fillRect(0,0,canvas.width, canvas.height)
        c.fillStyle = 'black';
        c.font = "30px monospace";
        c.textAlign = "center";
        c.fillText("Welcome To Alex and Travis' Game", canvas.width/2, 100);
        c.fillText("Press SPACE to continue", canvas.width/2, 200);
        addEventListener('keydown', ({ keyCode }) => {
            switch (keyCode) {
                case 32:
                    if (gamestarted == false) {
                        console.log('space');
                        gamestarted = true;
                        heart1.position.x = 500;
                        heart1.position.y = 40;
                        heart2.position.x = 540;
                        heart2.position.y = 40;
                        heart3.position.x = 580;
                        heart3.position.y = 40;
                        player.position.x = pSpawnX;
                        player.position.y = pSpawnY;
                        lives = 3;
                        enemyHealth = 3;
                        score = 0;
                        break;
                    }
            }
        });
    }
    // Render game objects and handle game logic
    // Include your game rendering and logic here
}
// Start the animation loop
animate();
        //--
        // NEW CODE - DRAW GENERIC OBJECTS WITH FOR EACH LOOP
        //--
        genericObjects.forEach(genericObject => {
            genericObject.draw()
        });
        // Draw platform, player, tube, and block object
        player.update();
        sword.update();
        enemy.update();
        platform.draw();
        c.fillStyle = "#000000aa";
        c.beginPath();
        c.roundRect(492.5,32.5,120,40,5);
        c.stroke();
        c.fill();
        heart1.update();
        heart2.update();
        heart3.update();
        //
        //Enemy AI
        if((player.position.x + player.width/2) > (enemy.position.x + enemy.width/2) && enemy.velocity.x < enemyCap){
            enemy.velocity.x += enemySpeed;
        }if((player.position.x + player.width/2) < (enemy.position.x + enemy.width/2) && enemy.velocity.x >-enemyCap){
            enemy.velocity.x -= enemySpeed;
        }
        c.fillStyle = "#000000aa";
        c.beginPath();
        c.roundRect(enemy.position.x + enemy.width/2 - 50/2, enemy.position.y - 10, 50, 7.5,5);
        c.stroke();
        c.fill();
        c.fillStyle = "lime";
        c.beginPath();
        c.roundRect(enemy.position.x + enemy.width/2 - 50/2, enemy.position.y - 9, 48 * (enemyHealth/3), 5, 5);
        c.stroke();
        c.fill();
        //Player damage
        if(isColliding(player, enemy)){
            const enemypos = (enemy.position.x + enemy.width/2);
            const playerpos = (player.position.x + player.width/2);
            //enemy.position.y = 200;
            //enemy.position.x = 500;
            player.velocity.y = -22.5;
            enemy.velocity.y = -20;
            if(enemypos > playerpos){
                console.log("Contact Left");
                player.velocity.x = -5;
                enemy.velocity.x = 10;
            }else if(enemypos <= playerpos){
                player.velocity.x = 5;
                enemy.velocity.x = -10;
                console.log("Contact Right");
            }
            score--;
            if(lives == 3){
                heart3.position.y = -45;
            }else if (lives == 2){
                heart2.position.y = -45
            }else if (lives == 1){
                heart1.position.y = -45;
                gamestarted = false;
            }
            lives--;
        }
        //Move sword;
        if(facing == true){
            sword.position.y = player.position.y - 2;
            sword.position.x = (player.position.x + player.width/2) + 15;
        }else if(facing == false){
            sword.position.y = player.position.y - 2;
            sword.position.x = (player.position.x + player.width/2) - 15;
        }
        // Score
        // Set the text content and position
        c.fillStyle = "#000000aa";
        c.beginPath();
        c.roundRect(45,25,125,40,5);
        c.stroke();
        c.fill();
        c.fillStyle = 'white';
        c.textAlign = 'left';
        c.font = "20px monospace";
        var text = "Score: "+score;
        var x = 50; // X-coordinate
        var y = 50; // Y-coordinate
        // Draw the text on the canvas
        ctx.fillText(text, x, y);
        //Collisions
        collision(platform, player);
        collision(platform, enemy);
        //collision(blockObject);
        //console.log(enemy.position);
        // Handle collisions and interactions
        // Handle collision between player and block object
        function collision(funcObject, objectToCollide){
            if (
                objectToCollide.position.y + objectToCollide.height <= funcObject.position.y &&
                objectToCollide.position.y + objectToCollide.height + objectToCollide.velocity.y >= funcObject.position.y &&
                objectToCollide.position.x + objectToCollide.width >= funcObject.position.x &&
                objectToCollide.position.x <= funcObject.position.x + funcObject.width
            )
            {
                objectToCollide.velocity.y = 0;
            }
        }
        function isColliding(spriteA, spriteB) {
            const collision =
                spriteA.position.x < spriteB.position.x + spriteB.width &&
                spriteA.position.x + spriteA.width > spriteB.position.x &&
                spriteA.position.y < spriteB.position.y + spriteB.height &&
                spriteA.position.y + spriteA.height > spriteB.position.y;
            return collision;
        }
        //prevent form going too high
        if(
            player.position.y + player.height <= 30
        ){
            player.velocity.y = 0;
            player.position.y = 30+player.height
        }
        // Move the player horizontally and adjust other objects
        if (keys.right.pressed && player.position.x < 500) {
            player.velocity.x = 15;
        }
        else if (keys.left.pressed && player.position.x > 100) {
            player.velocity.x = -15;
        }else if (player.velocity.y < 0 && player.position.x < 500 && player.position.x > 100){
        }
        //--
        // NEW CODE - PARALLAX SCROLLING EFFECT (MAKE THE BACKGROUND MOVE TO CREATE ILLUSION OF PLAYER MOVING)
        //--
        else {
            player.velocity.x = 0;
            if (keys.right.pressed && !keys.left.pressed) {
                // make the background move slower for a cooler effect
                genericObjects.forEach(genericObject => {
                    genericObject.position.x -= 5;
                });
                enemy.position.x -= 5;
            }
            else if (keys.left.pressed && !keys.right.pressed) {
                genericObjects.forEach(genericObject => {
                    genericObject.position.x += 5;
                });
                enemy.position.x += 5;
            }
        }
    // Start the animation loop
    animate();
    // Event listener for key presses
    addEventListener('keydown', ({ keyCode }) => {
        switch (keyCode) {
            case 65:
                console.log('left');
                keys.left.pressed = true;
                facing = false;
                break;
            case 83:
                console.log('down');
                break;
            case 68:
                console.log('right');
                keys.right.pressed = true;
                facing = true;
                break;
            case 87:
                console.log('up');
                if(player.velocity.y == 0){player.velocity.y = -20;}
                break;
            case 32:
                console.log('space');
                if (facing == false && player.position.x + player.width/2 - enemy.position.x + enemy.width/2 < 100 && player.position.x + player.width/2 - enemy.position.x + enemy.width/2 > 0 && player.position.y + player.height/2 - 10 < enemy.position.y + enemy.height/2 && player.position.y + player.height/2 + 10 > enemy.position.y + enemy.height/2){ //left
                    enemy.velocity.y = -20;
                    enemy.velocity.x = -5;
                    enemyHealth--;
                    console.log(player.position.x + player.width/2 - enemy.position.x + enemy.width/2);
                    if(enemyHealth == 0){
                        enemyHealth = 3;
                        enemy.position.x = 500;
                        enemy.position.y = 200;
                        score++;
                    }
                }else if (facing == true && enemy.position.x + enemy.width/2 - player.position.x + player.width/2 < 100 && enemy.position.x + enemy.width/2 - player.position.x + player.width/2 > 0 && player.position.y + player.height/2 - 10 < enemy.position.y + enemy.height/2 && player.position.y + player.height/2 + 10 > enemy.position.y + enemy.height/2){ //right
                    enemy.velocity.y = -20;
                    enemy.velocity.x = 5;
                    enemyHealth--;
                    console.log(enemy.position.x + enemy.width/2 - player.position.x + player.width/2);
                    if(enemyHealth == 0){
                        enemyHealth = 3;
                        enemy.position.x = 500;
                        enemy.position.y = 200;
                        score++;
                    }
                }
                break;
        }
    });
    // Event listener for key releases
    addEventListener('keyup', ({ keyCode }) => {
        switch (keyCode) {
            case 65:
                console.log('left');
                keys.left.pressed = false;
                player.velocity.x = 0;
                break;
            case 83:
                console.log('down');
                break;
            case 68:
                console.log('right');
                player.velocity.x = 0;
                keys.right.pressed = false;
                break;
            case 87:
                console.log('up');
                //if(player.velocity.y == 0){player.velocity.y = -20;}
                break;
        }
    });
