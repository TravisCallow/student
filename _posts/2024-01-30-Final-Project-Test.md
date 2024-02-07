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
     // Define the Platform class
    class Platform {
        constructor(image) {
            // Initial position of the platform
            this.position = {
                x: 0,
                y: 300
            }
            this.image = image;
            this.width = 650;
            this.height = 100;
        }
        // Method to draw the platform on the canvas
        draw() {
            c.drawImage(this.image, this.position.x, this.position.y, this.width, this.height);
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
    imageHills.src = 'https://samayass.github.io/samayaCSA/images/hills.png';
    // Create instances of platform, tube, block object, and generic objects
    let platform = new Platform(image);
    let tube = new Tube(imageTube);
    let blockObject = new BlockObject(imageBlock);
    //--
    // NEW CODE - CREATE ARRAY FOR GENERIC OBJECTS THEN ADD THE HILLS AND BACKGROUND
    //--
    let genericObjects = [
        new GenericObject({
            x:0, y:0, image: imageBackground
        }),
        new GenericObject({
            x:0, y:70, image: imageHills
        }),
    ];
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
