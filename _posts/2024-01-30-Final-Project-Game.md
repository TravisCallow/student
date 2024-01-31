---
toc: false
comments: false
layout: post
title: Week 7 Final Game
description: Cool Game
type: plans
courses: { compsci: {week: 7} }
---


 %%html
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
    // Define the Platform class
    class Platform {
        constructor() {
            // Initial position of the platform
            this.position = {
                x: 0,
                y: 300
            }
            //this.image = image;
            this.width = 650;
            this.height = 100;
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
    // Define the Tube class
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
    //--
    // NEW CODE - ADD IMAGES FOR BACKGROUND
    //--
    let imageBackground = new Image();
    let image = new Image();
    //--
    // NEW CODE - IMAGE URLS FOR BACKGROUND IMAGES
    //--
    // Create instances of platform, tube, block object, and generic objects
    let platform = new Platform();
    image.src = 'https://samayass.github.io/samayaCSA/images/platform.png';
    //let tube = new Tube(imageTube);
    //let blockObject = new BlockObject(imageBlock);
    //--
    // NEW CODE - CREATE ARRAY FOR GENERIC OBJECTS THEN ADD THE HILLS AND BACKGROUND
    //--
    let genericObjects = [
        new GenericObject({
            x:0, y:0, image: imageBackground
        }),
    ];
    player = new Player();
    sword = new Sword();
    // Define keys and their states
    let keys = {
        right: {
            pressed: false
        },
        left: {
            pressed: false
        }
    };
    // Animation loop
    function animate() {
        requestAnimationFrame(animate);
        c.clearRect(0, 0, canvas.width, canvas.height);
        //--
        // NEW CODE - DRAW GENERIC OBJECTS WITH FOR EACH LOOP
        //--
        genericObjects.forEach(genericObject => {
            genericObject.draw()
        });
        // Draw platform, player, tube, and block object
        player.update();
        sword.update();
        platform.draw();
        //
        //Move sword;
        if(facing == true){
            sword.position.y = player.position.y - 2;
            sword.position.x = (player.position.x + player.width/2) + 15;
        }else if(facing == false){
            sword.position.y = player.position.y - 2;
            sword.position.x = (player.position.x + player.width/2) - 15;
        }
        //Collisions
        collision(platform);
        //collision(blockObject);
        // Handle collisions and interactions
        // Handle collision between player and block object
        function collision(funcObject){
            if (
                player.position.y + player.height <= funcObject.position.y &&
                player.position.y + player.height + player.velocity.y >= funcObject.position.y &&
                player.position.x + player.width >= funcObject.position.x &&
                player.position.x <= funcObject.position.x + funcObject.width
            )
            {
                player.velocity.y = 0;
            }
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
            }
            else if (keys.left.pressed && !keys.right.pressed) {
                genericObjects.forEach(genericObject => {
                    genericObject.position.x += 5;
                });
            }
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
