---
toc: false
comments: false
layout: post
title: Week 7 Mario Game
description: A basic Mario level 
type: plans
courses: { compsci: {week: 7} }
---


 %%html
<style>
    #canvas {
        margin: 0;
        border: 1px solid white;
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
    player = new Player();
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
        platform.draw();
        player.update();
        tube.draw();
        blockObject.draw();
        //
        //Collisions
        collision(platform);
        collision(blockObject);
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
        // Handle interaction with tube
        if (
            player.position.y + player.height <= tube.position.y &&
            player.position.y + player.height + player.velocity.y >= tube.position.y &&
            player.position.x + player.width >= tube.position.x &&
            player.position.x <= tube.position.x + tube.width
        ) {
            player.velocity.y = 0;
            player.position.y += 0.1;
            player.velocity.y = 0.0001;
            gravity = 0.2;
        }
        // Adjust gravity after leaving the tube
        if (player.position.y + player.height == tube.position.y + tube.height ||
            player.position.y + player.height <= tube.position.y ||
            player.position.x + player.width <= tube.position.x ||
            player.position.x >= tube.position.x + tube.width) {
                gravity = 1.5;
            }
        // Handle collision with tube sides
        if (
            player.position.x + player.width<= tube.position.x &&
            player.position.x + player.width + player.velocity.x >= tube.position.x &&
            player.position.y + player.height >= tube.position.y &&
            player.position.y <= tube.position.y + tube.height
        )
        {
            player.velocity.x = 0;
        }
        if (
            player.position.x >= tube.position.x + tube.width &&
            player.position.x + player.velocity.x <= tube.position.x + tube.width &&
            player.position.y + player.height >= tube.position.y &&
            player.position.y <= tube.position.y + tube.height
        )
        {
            player.velocity.x = 0;
        }
        if (
            player.position.x >= tube.position.x &&
            player.position.x + player.velocity.x <= tube.position.x &&
            player.position.y + player.height >= tube.position.y &&
            player.position.y <= tube.position.y + tube.height
        )
        {
            player.velocity.x = 0;
        }
        if (
            player.position.x + player.width <= tube.position.x + tube.width &&
            player.position.x + player.width + player.velocity.x >= tube.position.x + tube.width &&
            player.position.y + player.height >= tube.position.y &&
            player.position.y <= tube.position.y + tube.height
        )
        {
            player.velocity.x = 0;
        }
        // Move the player horizontally and adjust other objects
        if (keys.right.pressed && player.position.x < 400) {
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
                platform.position.x -= 15;
                tube.position.x -= 15;
                blockObject.position.x -= 15;
                // make the background move slower for a cooler effect
                genericObjects.forEach(genericObject => {
                    genericObject.position.x -= 5;
                });
            }
            else if (keys.left.pressed && !keys.right.pressed) {
                platform.position.x += 15;
                tube.position.x += 15;
                blockObject.position.x += 15;
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
                break;
            case 83:
                console.log('down');
                break;
            case 68:
                console.log('right');
                keys.right.pressed = true;
                break;
            case 87:
                console.log('up');
                player.velocity.y -= 20;
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
                player.velocity.y = -20;
                break;
        }
    });
</script>
