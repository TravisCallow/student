---
toc: true
comments: false
layout: post
title: Sprite sheet animation
description: Sprite sheet animation
type: plans
courses: { compsci: {week: 5} }
---







<body>
    <div>
        <canvas id="spriteContainer"> <!-- Within the base div is a canvas. An HTML canvas is used only for graphics. It allows the user to access some basic functions related to the image created on the canvas (including animation) -->
            <img id="samuraiSprite" src="{{site.baseurl}}/images/samuri_animations-Recovered.png/">
        </canvas>
        <div id="controls"> <!--basic radio buttons which can be used to check whether each individual animaiton works -->
            <input type="radio" name="animation" id="idle" checked>
            <label for="idle">Idle</label><br>
            <input type="radio" name="animation" id="pulling out">
            <label for="pulling out">Pulling out</label><br>
            <input type="radio" name="animation" id="slicing">
            <label for="slicing">Slicing</label><br>
        </div>
</body>

<script>
    // start on page load
    window.addEventListener('load', function () {
        const canvas = document.getElementById('spriteContainer');
        const ctx = canvas.getContext('2d');
        const SPRITE_WIDTH = (768/6);  // matches sprite pixel width
        const SPRITE_HEIGHT = (192/3); // matches sprite pixel height
        const SCALE_FACTOR = 2;  // control size of sprite on canvas
        canvas.width = SPRITE_WIDTH * SCALE_FACTOR;
        canvas.height = SPRITE_HEIGHT * SCALE_FACTOR;

        class Samurai {
            constructor() {
                this.image = document.getElementById("samuraiSprite");
                this.x = 0;
                this.y = 0;
                this.minFrame = 0;
                this.maxFrame = 1;
                this.frameX = 0;
                this.frameY = 0;
            }

            // draw samurai object
            draw(context) {
                context.drawImage(
                    this.image,
                    this.frameX * SPRITE_WIDTH,
                    this.frameY * SPRITE_HEIGHT,
                    SPRITE_WIDTH,
                    SPRITE_HEIGHT,
                    this.x,
                    this.y,
                    canvas.width,
                    canvas.height
                );
            }

            // update frameX of object
            update() {
                if (this.frameX < this.maxFrame) {
                    this.frameX++;
                } else {
                    this.frameX = 0;
                }
            }
        }

        // samurai object
        const samurai = new Samurai();

        // update frameY of dog object, action from idle, bark, walk radio control
        const controls = document.getElementById('controls');
        controls.addEventListener('click', function (event) {
            if (event.target.tagName === 'INPUT') {
                const selectedAnimation = event.target.id;
                switch (selectedAnimation) {
                    case 'idle':
                        samurai.frameY = 0;
                        samurai.maxFrame = 1;
                        break;
                    case 'pulling out':
                        samurai.frameY = 1;
                        samurai.maxFrame = 4;
                        break;
                    case 'slicing':
                        samurai.frameY = 2;
                        samurai.maxFrame = 5;
                        break;
                    default:
                        break;
                }
            }
        });

        // Animation recursive control function
        function animate() {
            // Clears the canvas to remove the previous frame.
            ctx.clearRect(0, 0, canvas.width, canvas.height);

            // Draws the current frame of the sprite.
            samurai.draw(ctx);

            // Updates the `frameX` property to prepare for the next frame in the sprite sheet.
            samurai.update();

            // Uses `requestAnimationFrame` to synchronize the animation loop with the display's refresh rate,
            // ensuring smooth visuals.
            setTimeout(()=>{requestAnimationFrame(animate);}, 250);
        }

        // run 1st animate
        animate();
    });
</script>
