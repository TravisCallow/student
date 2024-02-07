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
        background: skyblue;
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
    // Set gravity value
    let gravity = 1.5;
    // Facing Value | true = right, false = left
    let facing = false;
    // Score
    let score = 0;
    // Define the Player class
    class Player {
        constructor() {
            // Initial position and velocity of the player
            this.position = {
                x: 100,
                y: 200
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
            c.fillStyle = 'red';
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
    // Make Sword
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
        // Method to draw the sword on the canvas
        draw() {
            c.fillStyle = 'purple';
            c.fillRect(this.position.x, this.position.y, this.width, this.height);
        }
        // Method to update the sword position and velocity
        update() {
            this.draw();
        }
    }
    class PlayerAnimation {
        constructor() {
            this.sprite = new Image();
            // Update the image source URL as needed
            this.sprite.src = '{{site.baseurl}}/images/samuri_animations-Recovered.png';
            this.frameX = 0;
            this.frameY = 1;
            this.width = 30; // Adjust the width based on your sprite sheet
            this.height = 30; // Adjust the height based on your sprite sheet
        }
        draw(player) {
            c.drawImage(
                this.sprite,
                this.frameX * this.width,
                this.frameY * this.height,
                this.width,
                this.height,
                player.position.x,
                player.position.y,
                this.width,
                this.height
            );
        }
        update() {
            this.frameX++;
            if (this.frameX >= 4) {
                this.frameX = 0;
            }
        }
    }
    // Create instances for player, sword, and player animation
    let player = new Player();
    let sword = new Sword();
    let playerAnimation = new PlayerAnimation();
// Animation loop
function animate() {
    setTimeout(function () {
        requestAnimationFrame(animate);
        c.clearRect(0, 0, canvas.width, canvas.height);
        // Draw player with animation
        playerAnimation.draw(player);
        // Update player animation frame
        playerAnimation.update();
        // Draw other objects (sword, platform, etc.)
        // player.update();
        // sword.update();
        // platform.draw();
        // ...
        // Handle other game logic 
        // ...
    }, 1000 / 10); // Adjust the delay (1000 milliseconds / frames per second)
}
// Start the animation loop
animate();
    // Event listener for key presses
    document.addEventListener('keydown', ({ keycode }) => {
        switch (keycode) {
            case 65:
                console.log('left');
                facing = false;
                break;
            case 83:
                console.log('down');
                break;
            case 68:
                console.log('right');
                facing = true;
                break;
            case 87:
                console.log('up');
                if(player.velocity.y == 0) {
                    player.velocity.y = -20;
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
                break;
            case 83:
                console.log('down');
                break;
            case 68:
                console.log('right');
                keys.right.pressed = false;
                break;
            case 87:
                console.log('up');
                //if(player.velocity.y == 0){player.velocity.y = -20;}
                break;
        }
    });
</script>
