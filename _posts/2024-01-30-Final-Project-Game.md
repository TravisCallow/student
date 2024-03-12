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
    canvas {
  border: 1px solid black;
}
</style>
<canvas width="300" height="200"></canvas>
<script>
    let img = new Image();    
img.src = '{{site.baseurl}}/images/Green-Cap-Character-16x18.png';
function init() {
    ctx.drawImage(img, 0, 0, 16, 18, 0, 0, 16, 18);
}
//img.src = '{{site.baseurl}}/images/Green-Cap-Character-16x18.png';    
</script>