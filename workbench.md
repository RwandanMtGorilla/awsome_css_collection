


####
我注意到一篇教程 内容如下:

纯 CSS 实现超酷炫的粘性气泡效果

```html
div.main
div.footer
    div.bubbles
        - for (var i = 0; i < 128; i++) //Small numbers looks nice too
            div.bubble(style=`--size:${2+Math.random()*4}rem; --distance:${6+Math.random()*4}rem; --position:${-5+Math.random()*110}%; --time:${2+Math.random()*2}s; --delay:${-1*(2+Math.random()*2)}s;`)
    div.content
        div
            div
                b Eldew
                a(href="#") Secuce
                a(href="#") Drupand
                a(href="#") Oceash
                a(href="#") Ugefe
                a(href="#") Babed
            div
                b Spotha
                a(href="#") Miskasa
                a(href="#") Agithe
                a(href="#") Scesha
                a(href="#") Lulle
            div
                b Chashakib
                a(href="#") Chogauw
                a(href="#") Phachuled
                a(href="#") Tiebeft
                a(href="#") Ocid
                a(href="#") Izom
                a(href="#") Ort
            div
                b Athod
                a(href="#") Pamuz
                a(href="#") Vapert
                a(href="#") Neesk
                a(href="#") Omemanen
        div
            a.image(href="https://codepen.io/z-" target="_blank" style="background-image: url(\"https://s3-us-west-2.amazonaws.com/s.cdpn.io/199011/happy.svg\")")
            p ©2019 Not Really
svg(style="position:fixed; top:100vh")
    defs
        filter#blob
            feGaussianBlur(in="SourceGraphic" stdDeviation="10" result="blur")
            feColorMatrix(in="blur" mode="matrix" values="1 0 0 0 0  0 1 0 0 0  0 0 1 0 0  0 0 0 19 -9" result="blob")
            //feComposite(in="SourceGraphic" in2="blob" operator="atop") //After reviewing this after years I can't remember why I added this but it isn't necessary for the blob effect
```

```css
    body {
    display:grid;
    grid-template-rows: 1fr 10rem auto;
    grid-template-areas:"main" "." "footer";
    overflow-x:hidden;
    background:#F5F7FA;
    min-height:100vh;
    font-family: 'Open Sans', sans-serif;
    .footer {
        z-index: 1;
        --footer-background:#ED5565;
        display:grid;
        position: relative;
        grid-area: footer;
        min-height:12rem;
        .bubbles {
            position: absolute;
            top:0;
            left:0;
            right:0;
            height:1rem;
            background:var(--footer-background);
            filter:url("#blob");
            .bubble {
                position: absolute;
                left:var(--position, 50%);
                background:var(--footer-background);
                border-radius:100%;
                animation:bubble-size var(--time, 4s) ease-in infinite var(--delay, 0s),
                    bubble-move var(--time, 4s) ease-in infinite var(--delay, 0s);
                transform:translate(-50%, 100%);
            }
        }
        .content {
            z-index: 2;
            display:grid;
            grid-template-columns: 1fr auto;
            grid-gap: 4rem;
            padding:2rem;
            background:var(--footer-background);
            a, p {
                color:#F5F7FA;
                text-decoration:none;
            }
            b {
                color:white;
            }
            p {
                margin:0;
                font-size:.75rem;
            }
            >div {
                display:flex;
                flex-direction:column;
                justify-content: center;
                >div {
                    margin:0.25rem 0;
                    >* {
                        margin-right:.5rem;
                    }
                }
                .image {
                    align-self: center;
                    width:4rem;
                    height:4rem;
                    margin:0.25rem 0;
                    background-size: cover;
                    background-position: center;
                }
            }
        }
    }
}

@keyframes bubble-size {
    0%, 75% {
        width:var(--size, 4rem);
        height:var(--size, 4rem);
    }
    100% {
        width:0rem;
        height:0rem;
    }
}
@keyframes bubble-move {
    0% {
        bottom:-4rem;
    }
    100% {
        bottom:var(--distance, 10rem);
    }
}

```


如何让气泡与气泡之间，以及气泡和底部 .g-footer 之间产生融合效果呢？

这个技巧就是利用 filter: contrast() 滤镜与 filter: blur() 滤镜。

简述下该技巧：
单独将两个滤镜拿出来，它们的作用分别是：
filter: blur()： 给图像设置高斯模糊效果。
filter: contrast()： 调整图像的对比度。

我现在有一份代码 是用html做鸟群算法的模拟 如下:
index.html
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="utf-8">
	<title>BOIDS!</title>
	<link rel="stylesheet" href="css/style.css">
	<!--[if IE]>
		<script src="http://html5shiv.googlecode.com/svn/trunk/html5.js"></script>
	<![endif]-->
</head>

<body id="home">
  <div id="boids-wrapper">
    <canvas id="boids"></canvas>
	<div id="boids-controls-container" >
		<div id="mobile-boids-controls">
			<button id="introversion-mobile">Introversion</button>
			<button id="speed-mobile">Speed</button>
			<button id="walls-mobile" class="boids-checkbox-on">Walls</button>
			<button id="collisions-mobile">Collisions</button>
			<button id="mouse-seek-mobile">Seek Mouse</button>
			<button id="racism-mobile">Racism</button>
			<button id="diversity-mobile">Diversity</button>
		</div>
		<div id="introversion-control-container" class="boids-control boids-control-range">
			<span class="boids-control-close"></span>
			<div class="range-slider">
				<label for="introversion"><p>Introversion</p></label>
				<input class="input-range" type="range" step="1" value="1" min="0" max="10" name="introversion" id="introversion">
			<span class="range-value"></span>
			</div>
		</div>
		<div id="speed-control-container" class="boids-control boids-control-range">
			<span class="boids-control-close"></span>
			<div class="range-slider">
				<label for="introversion"><p>Speed</p></label>
				<input class="input-range" type="range" step="1" value="1" min="0" max="10" name="speed" id="speed">
			<span class="range-value"></span>
			</div>
		</div>
		<div class="boids-control boids-control-checkbox">
			<div class="checkbox">
				<p>Walls</p>
				<input type="checkbox" id="walls" name="walls"/>
				<label for="walls"></label>
			</div>
		</div>
		<div class="boids-control boids-control-checkbox">
			<div class="checkbox">
				<p>Collisions</p>
				<input type="checkbox" id="collision-detection" name="collision-detection"/>
				<label for="collision-detection"></label>
			</div>
		</div>
		<div class="boids-control boids-control-checkbox">
			<div class="checkbox">
				<p>Seek Mouse</p>
				<input type="checkbox" id="mouse-seek" name="mouse-seek"/>
				<label for="mouse-seek"></label>
			</div>
		</div>
		<div id="racism-control-container" class="boids-control boids-control-range">
			<span class="boids-control-close"></span>
			<div class="range-slider">
				<label for="introversion"><p>Racism</p></label>
				<input class="input-range" type="range" step="1" value="1" min="0" max="10" name="racism" id="racism">
			<span class="range-value"></span>
			</div>
		</div>
		<div id="diversity-control-container" class="boids-control boids-control-range">
			<span class="boids-control-close"></span>
			<div class="range-slider">
				<label for="introversion"><p>Diversity</p></label>
				<input class="input-range" type="range" step="1" value="8" min="1" max="8" name="diversity" id="diversity">
			<span class="range-value"></span>
			</div>
		</div>
		<div id="fps">
			<p><span id="fps-number"></span> fps</p>
		</div>
	</div>
	</div>

  <script src="js/victor.min.js"></script>
  <script src="js/boid.js"></script>
  <script src="js/script.js"></script>
</body>
</html>


```

boid.js
```js
class Boid {

  /**
   * Contstruct Boid object
   *
   * @param  object | boid | Initial setup properties for boid
   *
   */
  constructor(boid) {

    // Initial Properties
    this.id = boid.id;
    this.position = new Victor( boid.x, boid.y );
    this.radius = boid.radius * radiusCoefficients[ boid.radiusCoefficient ];
    this.introversionCoefficient = boid.introversionCoefficient;
    this.introversion = boid.introversion * this.introversionCoefficient;
    this.quicknessCoefficient = boid.quicknessCoefficient;
    this.quickness = boid.quickness * this.quicknessCoefficient;
    this.racismCoefficient = boid.racismCoefficient;
    this.racism = boid.racism * boid.racismCoefficient;
    this.color = boid.color;
    this.mass = (4/3) * Math.PI * Math.pow( this.radius,3 );

    // Speed & Velocity & Force
    this.maxSpeed = speedIndex * this.quickness;
    this.speed = this.maxSpeed * .5;
    var radians = Math.PI * getRandomInt(-99,100) / 100;
    this.velocity = new Victor( this.speed * Math.cos( radians ), this.speed * Math.sin( radians ) );
    //Force and Accel
    this.maxForce = .5;

  }

  /**
   * Calculate the seek force for a boid and a target
   *
   * @param  object | target | The Victor.js vector for a target's position
   * @return object | The seek force for the target as a vector
   */
  seek( target ){
    var targetposition = target.clone();
    var diff = targetposition.subtract(this.position);
    var desired = new Victor(diff.x,diff.y);

    if (target.radius) {
      var buffer = target.radius + this.radius + 1;
    } else {
      var buffer = this.radius * 2 + 1;
    }

    var dist = diff.magnitude();
    if (dist < buffer) {
      desired.x = 0;
      desired.y = 0;
    } else if ( dist <= 100 ) {
      desired.normalize();
      desired.divide({x:this.maxSpeed * dist / 100,y:this.maxSpeed * dist / 100});
    } else {
      desired.limitMagnitude(this.maxSpeed);
    }
    desired.subtract(this.velocity);
    desired.limitMagnitude(this.maxForce);
    return desired;
  }

  /**
   * Calculate the separation force for a boid and its flock
   *
   * @param  array | boids | The boids array containing all the boids
   * @return object | The Separation force as a Victor vector
   */
  separate( boids ){
    var sum = new Victor();
    var count = 0;
    for (var j = 0; j < boids.length; j++) {
      if ( this.color != boids[j].color ) {
        var racismMultiplier = this.racism;
      } else {
        var racismMultiplier = 0;
      }
      var desiredSeparation = this.radius + boids[j].radius + ( 25 * this.introversion ) + ( 50 * racismMultiplier );
      var sep = this.position.clone().distance(boids[j].position);
      if ( (sep > 0) && (sep < desiredSeparation) ) {
        var thisposition = this.position.clone();
        var diff = thisposition.subtract(boids[j].position);
        diff.normalize();
        diff.divide({x:sep,y:sep});
        sum.add(diff);
        count++;
      }
    }
    if (count > 0) {
      sum.divide({x:count,y:count});
      sum.normalize();
      sum.multiply({x:this.maxSpeed,y:this.maxSpeed});
      sum.subtract(this.velocity);
      sum.limitMagnitude(this.maxForce);
    }
    return sum;
  }

  /**
   * Calculate the alignment force for a boid and its flock
   *
   * @param  array | boids | The boids array containing all the boids
   * @return object | The alignment force as a Victor vector
   */
  align( boids ) {
    var neighborDist = 50;
    var sum = new Victor();
    var steer = new Victor();
    var count = 0;
    for (var i = 0; i < boids.length; i++) {
      var dist = this.position.distance(boids[i].position);
      if ( dist > 0 && dist < neighborDist ) {
        sum.add(boids[i].velocity);
        count++;
      }
    }
    if (count > 0) {
      sum.divide({x:count,y:count});
      sum.normalize()
      sum.multiply({x:this.maxSpeed,y:this.maxSpeed});
      steer = sum.subtract(this.velocity);
      steer.limitMagnitude(this.maxForce);
      return steer;
    } else {
      return steer;
    }
  }

  /**
   * Calculate the cohesion force for a boid and its flock
   *
   * @param  array | boids | The boids array containing all the boids
   * @return object | The cohesion force as a Victor vector
   */
  cohesion( boids ) {
    var neighborDist = 50;
    var sum = new Victor();
    var count = 0;
    for (var i = 0; i < boids.length; i++) {
      var dist = this.position.distance(boids[i].position);
      if ( dist > 0 && dist < neighborDist ) {
        sum.add(boids[i].position);
        count++;
      }
    }
    if (count > 0) {
      sum.divide({x: count,y:count});
      return this.seek(sum);
    } else {
      return sum;
    }
  }

  /**
   * Avoid the canvas walls if walls are enabled
   *
   * @return object/boolean | The seek force to avoid a wall, or false if not near a wall
   */
  avoidWalls() {

    var buffer = mobile ? 5 : 15;

    if ( this.distanceFromHorWall() < this.radius * buffer || this.distanceFromVertWall() < this.radius * buffer ) {
      return this.seek(center);
    } else { return false; }

  }

  /**
   * Run force calculation functions for the boid, then apply forces
   *
   */
  flock() {

    // Get Forces
    var alignForce = this.align(boids);
    if ( mouseSeek ) var mouseForce = this.seek(mouse.position);
    var separateForce = this.separate(boids);
    var cohesionForce = this.cohesion(boids);
    if ( walls ) var avoidWallsForce = this.avoidWalls();

    // Weight Forces
    var alignWeight = 1.2;
    if ( mouseSeek ) var mouseWeight = .2;
    var separateWeight = 1;
    var cohesionWeight = 1;
    if ( walls ) var avoidWallsWeight = 1.2;


    // Apply forces
    this.applyForce( alignForce, alignWeight );
    if ( mouseSeek ) this.applyForce( mouseForce, mouseWeight );
    this.applyForce( separateForce, separateWeight );
    this.applyForce( cohesionForce, cohesionWeight );
    if ( walls && avoidWallsForce ) this.applyForce( avoidWallsForce, avoidWallsWeight );

  }

  /**
   * Apply a coefficient to a given force and apply it to the boid
   *
   * @param object | force | The Victor vector of the force to be applied
   * @param float | coefficient | The factor to be applied to the force
   */
  applyForce( force, coefficient ) {
    if ( ! coefficient ) { var coefficient = 1; }
    force.multiply({x:coefficient,y:coefficient});
    this.velocity.add(force);
    this.velocity.limitMagnitude( this.maxSpeed );
  }

  /**
   * Run the flock function and update the boid's position based on the resulting velocity
   *
   */
  nextPosition() {

    // Loop through behaviors to apply forces
    this.flock();

    // Update position
    this.position = this.position.add(this.velocity);

    // Collision detection if enabled
    if ( collisions ) { this.detectCollision(); }

    // Check edges for walls or overruns
    this.edgeCheck();

  }

  /**
   * Check for edge crossings and bounce the boid or wrap it to the other side of the canvas
   *
   */
  edgeCheck() {
    if (walls) {
      this.wallBounce();
    } else {
      this.borderWrap();
    }
  }

  /**
   * If the boid passes a border with no walls, wrap the boid to the other side of the canvas
   *
   */
  borderWrap() {
    if (this.position.x < 0) {
      this.position.x = document.body.clientWidth;
    } else if ( this.position.x > document.body.clientWidth ) {
      this.position.x = 0;
    }
    if (this.position.y < 0) {
      this.position.y = document.body.clientHeight;
    } else if ( this.position.y > document.body.clientHeight ) {
      this.position.y = 0;
    }
  }

  /**
   * Detect a wall hit and bounce boid
   *
   */
  wallBounce() {
    if (this.position.x <= this.radius) {
      this.position.x = this.radius;
    } else if ( this.position.x >= document.body.clientWidth - this.radius) {
      this.position.x = document.body.clientWidth - this.radius;
    }
    if (this.position.y <= this.radius) {
      this.position.y = this.radius;
    } else if ( this.position.y >= document.body.clientHeight - this.radius ) {
      this.position.y = document.body.clientHeight - this.radius;
    }
    if ( this.distanceFromHorWall() <= this.radius  ) {
      this.velocity.invertY();
    }
    if ( this.distanceFromVertWall() <= this.radius  ) {
      this.velocity.invertX();
    }
  }

  /**
   * Calculate the distance from vertical wall in the direction of the boid's velocity
   *
   * @param float | the boid's distance from the wall
   */
  distanceFromVertWall() {
    if (this.velocity.x > 0) {
      return document.body.clientWidth - ( this.position.x );
    } else {
      return this.position.x;
    }

  }

  /**
   * Calculate the distance from horizontal wall in the direction of the boid's velocity
   *
   * @param float | the boid's distance from the wall
   */
  distanceFromHorWall() {
    if (this.velocity.y > 0) {
      return document.body.clientHeight - ( this.position.y );
    } else {
      return this.position.y;
    }
  }

  /**
   * Draw Boid to the canvas
   *
   */
  draw(){
    c.beginPath();
    c.arc(this.position.x, this.position.y, this.radius, 0, Math.PI * 2, false);
    c.fillStyle = this.color;
    c.fill();
    c.closePath();
  }

  /**
   * Update a boid's position and call draw()
   *
   */
  update() {

    this.nextPosition();
    this.draw();

  }

  /**
   * Detect collisions between boids and resolve
   *
   */
  detectCollision(){

    for (var i = 0; i < boids.length; i++) {
      if ( this === boids[i] ) { continue; }
      if ( getDistance( this.position.x, this.position.y, boids[i].position.x, boids[i].position.y) - ( this.radius + boids[i].radius ) < 0 ) {
        this.resolveCollision( this, boids[i]);
      }
    }
  }

/**
 * Rotates coordinate system for velocities
 * Takes velocities and alters them as if the coordinate system they're on was rotated
 *
 * @param  object | velocity | The velocity of an individual boid
 * @param  float  | angle    | The angle of collision between two objects in radians
 * @return object | The altered x and y velocities after the coordinate system has been rotated
 */
  rotate(velocity, angle) {
    return {
        x: velocity.x * Math.cos(angle) - velocity.y * Math.sin(angle),
        y: velocity.x * Math.sin(angle) + velocity.y * Math.cos(angle)
    };
  }

  /**
   * Swaps out two colliding boids' x and y velocities after running through
   * an elastic collision reaction equation
   *
   * @param  object | boid      | A boid object
   * @param  object | otherBoid | A boid object
   */
   resolveCollision(boid, otherBoid) {

      var xVelocityDiff = boid.velocity.x - otherBoid.velocity.x;
      var yVelocityDiff = boid.velocity.y - otherBoid.velocity.y;

      var xDist = otherBoid.position.x - boid.position.x;
      var yDist = otherBoid.position.y - boid.position.y;

      // Prevent accidental overlap of boids
      if ( xVelocityDiff * xDist + yVelocityDiff * yDist >= 0 ) {

        // Grab angle between the two colliding boids
        var angle = -Math.atan2(otherBoid.position.y - boid.position.y, otherBoid.position.x - boid.position.x);

        // Store mass in var for better readability in collision equation
        var m1 = boid.mass;
        var m2 = otherBoid.mass;

        // Velocity before equation
        var u1 = this.rotate(boid.velocity, angle);
        var u2 = this.rotate(otherBoid.velocity, angle);

        // Velocity after 1d collision equation
        var v1 = { x: u1.x * (m1 - m2) / (m1 + m2) + u2.x * 2 * m2 / (m1 + m2), y: u1.y };
        var v2 = { x: u2.x * (m1 - m2) / (m1 + m2) + u1.x * 2 * m2 / (m1 + m2), y: u2.y };

        // Final velocity after rotating axis back to original position
        var vFinal1 = this.rotate(v1, -angle);
        var vFinal2 = this.rotate(v2, -angle);

        // Swap boid velocities for realistic bounce effect
        boid.velocity.x = vFinal1.x;
        boid.velocity.y = vFinal1.y;
        boid.velocity.limitMagnitude(boid.maxSpeed);

        otherBoid.velocity.x = vFinal2.x;
        otherBoid.velocity.y = vFinal2.y;
        otherBoid.velocity.limitMagnitude(otherBoid.maxSpeed);
      }

    }

}


```

script.js
```js
/*
 *  JumpOff JavaScript Boids
 *
 *  Copyright 2017, JumpOff, LLC
 *  Github:  https://github.com/jqlee85/boids
 *  Author: JumpOff, LLC
 *  Author URL: https://jumpoff.io
 *
 *  License: WTFPL license
 *  License URL: http://sam.zoy.org/wtfpl/
 *
 *  Version: 1.0
 */

/*---- Global Setup ----*/

// Set up canvas
const canvas = document.getElementById('boids');
const c = canvas.getContext('2d');

// Get Firefox
var browser=navigator.userAgent.toLowerCase();
if(browser.indexOf('firefox') > -1) {
  var firefox = true;
}

// Detect Mobile
var mobile = ( /Android|webOS|iPhone|iPad|iPod|BlackBerry|IEMobile|Opera Mini/i.test(navigator.userAgent) ) ? true : false;

// Set Size
var size = {
  width: window.innerWidth || document.body.clientWidth,
  height: window.innerHeight || document.body.clientHeight
}
canvas.width = size.width;
canvas.height = size.height;
var center = new Victor( size.width / 2 ,size.height / 2 );

// Initialize Mouse
var mouse = {
  position: new Victor( innerWidth / 2, innerHeight / 2 )
};

/*---- end Global Setup ----*/

/*---- Helper Functions ----*/

/**
 * Returns a random int between a min and a max
 *
 * @param  int | min | A minimum number
 * @param  int | max | A maximum number
 * @return int | The random number in the given range
 */
function getRandomInt(min, max) {
  return Math.floor(Math.random() * (max - min + 1)) + min;
}

/**
 * Returns the distance between two coordinates
 *
 * @param  int | x1 | Point 1's x coordinate
 * @param  int | y1 | Point 1's y coordinate
 * @param  int | x2 | Point 2's x coordinate
 * @param  int | y2 | Point 2's y coordinate
 * @return int | The distance between points 1 and 2
 */
function getDistance(x1, y1, x2, y2) {
  var xDist = x2 - x1;
  var yDist = y2 - y1;
  return Math.sqrt( Math.pow(xDist, 2) + Math.pow(yDist, 2) );
}

/**
 * Returns a random color from the colors array
 *
 * @param  array | colors | An array of color values
 * @return string | The random color value
 */
function randomColor(colors) {
  return colors[ Math.floor( Math.random() * colors.length) ];
}

/**
 * Get coefficients based on normal distribution
 *
 * @param  int | mean | The mean value of the data set
 * @param  int | stdev | The standard deviation for the data set
 * @return int | A number from the data set
 */
function gaussian(mean, stdev) {
    var y2;
    var use_last = false;
    return function() {
        var y1;
        if(use_last) {
           y1 = y2;
           use_last = false;
        }
        else {
            var x1, x2, w;
            do {
                 x1 = 2.0 * Math.random() - 1.0;
                 x2 = 2.0 * Math.random() - 1.0;
                 w  = x1 * x1 + x2 * x2;
            } while( w >= 1.0);
            w = Math.sqrt((-2.0 * Math.log(w))/w);
            y1 = x1 * w;
            y2 = x2 * w;
            use_last = true;
       }

       var retval = mean + stdev * y1;
       if(retval > 0)
           return retval;
       return -retval;
   }
}
var getCoefficient = gaussian(50, 9);
var getQuicknessCoefficient = gaussian(75,7.5);

/**
 * Add Limit Magnitude function to Victor objects
 *
 * @param  int | max | The limit magnitude for the vector
 */
Victor.prototype.limitMagnitude = function (max) {

  if (this.length() > max) {
    this.normalize();
    this.multiply({x:max,y:max});
  }

};

/*--- end Helper Functions ----*/

/*---- Loop and Initializing ----*/

// Checkbox Options
var walls = false;
var mouseSeek = true;
var collisions = false;

// Set number of boids based on browser and screen size
if (firefox) {
  var maxBoids = 250;
} else if (mobile) {
  var maxBoids = 150;
} else {
  var maxBoids = 500;
}
var minBoids = 250;
var numBoids = Math.sqrt(canvas.width * canvas.height) / 2;
if ( numBoids > maxBoids ) {
  numBoids = maxBoids;
} else if ( numBoids < minBoids ) {
  numBoids = minBoids;
}

// Set possible radii  based on screen size
var radius;
if ( size.width / 288 > 5 ) {
  radius = 15;
} else if ( size.width / 288 < 3) {
  radius = 9;
} else {
  radius = size.width / 288 * 3;
}
var radiusCoefficients = [.5,.6,.7,.8,1];

// Boid Attributes
// var colors = [
//   '#4286f4',
//   '#f4416a',
//   '#41f4a0',
//   '#f9f9f9',
//   '#a341f4',
//   '#f48341',
//   '#f4e841',
//   '#42ebf4'
// ];

var colors = ['#5553FF', '#5A5CFF', '#9270E3', '#6E80FF', '#7389FF','#6666FF','#9900FF','#B221FB'];


var diversity = 8;
var quickness = 1;
var introversion = .5;
var racism = 1;
var speedIndex;
if ( size.width / 160 < 5 ) {
  speedIndex = 5;
} else if ( size.width / 180 > 8 ) {
  speedIndex = 9;
} else {
  speedIndex = size.width / 180;
}

// Create Boids Array
var boids = [];

/**
 * Create Boids Array
 *
 */
function createBoids() {

  // Instantiate all Boids
  for ( i = 0; i < numBoids; i++ ) {

    // Generate introversion coefficient
    var introversionCoefficient = getCoefficient() / 100;
    var quicknessCoefficient = getQuicknessCoefficient() / 100;
    var racismCoefficient = getCoefficient() / 100;
    var radiusCoefficient = Math.floor(Math.random() * radiusCoefficients.length);

    // Generate random coords
    var x = Math.ceil(Math.random()* ( size.width - ( radius * 2 ) ) ) + ( radius );
    var y = Math.ceil(Math.random()* ( size.height - ( radius * 2 ) ) ) + ( radius );
    // For subsequent boids, check for collisions and generate new coords if exist
    if ( i !== 0 ) {
      for (var j = 0; j < boids.length; j++ ) {
        if ( getDistance(x, y, boids[j].x, boids[j].y) - ( radius + boids[j].radius ) < 0 ) {
          x = Math.ceil(Math.random()* ( size.width - ( radius * 2 ) ) ) + ( radius );
          y = Math.ceil(Math.random()* ( size.height - ( radius * 2 ) ) ) + ( radius );
          j = -1;
        }
      }
    }

    // Add new Boid to array
    boids.push( new Boid( {
      id: i,
      x: x,
      y: y,
      speedIndex: speedIndex,
      radius: radius,
      radiusCoefficient: radiusCoefficient,
      quickness: quickness,
      quicknessCoefficient: quicknessCoefficient,
      color: randomColor(colors),
      racism: racism,
      racismCoefficient: racismCoefficient,
      introversion: introversion,
      introversionCoefficient: introversionCoefficient
    } ) );
  }

}

/**
 * Setup and call animation function
 *
 */
function animate() {
  requestAnimationFrame(animate);

  // Calc elapsed time since last loop
  now = Date.now();
  elapsed = now - then;

  // FPS Reporting
  fpsReport++;
  if (fpsReport > 60) {
    fpsNum.innerHTML = Math.floor(1000/elapsed);
    fpsReport = 0;
  }

  // If enough time has elapsed, draw the next frame
  if (elapsed > fpsInterval) {
      // Get ready for next frame by setting then=now, but also adjust for your
      // specified fpsInterval not being a multiple of RAF's interval (16.7ms)
      then = now - (elapsed % fpsInterval);
      // Drawing Code
      c.clearRect(0, 0, canvas.width, canvas.height);
      // Update all boids
      for (var i = 0; i < boids.length; i++ ) {
        boids[i].update();
      }
  }
}

// Setup animation
var stop = false;
var frameCount = 0;
var fps, fpsInterval, startTime, now, then, elapsed;
var fpsNum = document.getElementById('fps-number');
var fpsReport = 58;

/**
 * Start Animation of Boids
 *
 */
function startAnimating() {
  if(fps == null) { var fps = 60; }
  fpsInterval = 1000 / fps;
  then = Date.now();
  startTime = then;
  animate();
}

//Initalize program
createBoids();
startAnimating(60);

/*---- end Loop and Initializing ----*/

/*---- Event Listeners ----*/

/**
 * Update mouse positions on mousemove
 *
 */
addEventListener('mousemove', function(event){
  mouse.position.x = event.clientX;
  mouse.position.y = event.clientY;
});

/**
 * Update boundary sizes on window resize
 *
 */
addEventListener('resize', function(){
  size.width = innerWidth;
  size.height = innerHeight;
  canvas.width = innerWidth;
  canvas.height = innerHeight;
  center.x = size.width/ 2;
  center.y = size.height / 2;
  if ( innerWidth >= 1000 && ! mobile ) {
    document.getElementById('mobile-boids-controls').style.display = 'none';
  } else {
    document.getElementById('mobile-boids-controls').style.display = 'block';
  }
});

/*---- end Event Listeners ----*/

/*---- Inputs ----*/

// Hide Elements on Mobile
document.getElementById('collisions-mobile').style.display = 'none';
document.getElementById('mouse-seek-mobile').style.display = 'none';

// Mobile Closers
var mobileClosers = document.getElementsByClassName('boids-control-close');
for (var i = 0; i < mobileClosers.length; i++) {
  mobileClosers[i].onclick = function() {
    this.parentNode.classList.toggle('show');
    document.getElementById('mobile-boids-controls').style.display = 'block';
  }
}

// Walls
var wallsInput = document.getElementById('walls');
wallsInput.checked = false;
wallsInput.onclick = function() {
  if ( !this.checked ) {
    this.checked = false;
    wallsMobile.dataset.checked = false;
    wallsMobile.classList.toggle('boids-checkbox-on');
    walls = false;
  } else {
    this.checked = true;
    wallsMobile.dataset.checked = true;
    wallsMobile.classList.toggle('boids-checkbox-on');
    walls = true;
  }
}
var wallsMobile = document.getElementById('walls-mobile');
wallsMobile.dataset.checked = false;
wallsMobile.onclick = function() {
  if ( this.dataset.checked == 'false') {
    this.dataset.checked = true;
    wallsInput.checked = true;
    this.classList.toggle('boids-checkbox-on');
    walls = true;
  } else {
    this.dataset.checked = false;
    wallsInput.checked = false;
    this.classList.toggle('boids-checkbox-on');
    walls = false;
  }
}

// Collision Detection
var collisionDetectionInput = document.getElementById('collision-detection');
collisionDetectionInput.checked = false;
collisionDetectionInput.onclick = function() {
  if ( !this.checked ) {
    this.checked = false;
    collisionDetectionMobile.dataset.checked = false;
    collisionDetectionMobile.classList.toggle('boids-checkbox-on');
    collisions = false;
  } else {
    this.checked = true;
    collisionDetectionMobile.dataset.checked = true;
    collisionDetectionMobile.classList.toggle('boids-checkbox-on');
    collisions = true;
  }
}
var collisionDetectionMobile = document.getElementById('collisions-mobile');
collisionDetectionMobile.dataset.checked = false;
collisionDetectionMobile.onclick = function() {
  if ( this.dataset.checked == 'false') {
    this.dataset.checked = true;
    collisionDetectionInput.checked = true;
    this.classList.toggle('boids-checkbox-on');
    collisions = true;
  } else {
    this.dataset.checked = false;
    collisionDetectionInput.checked = false;
    this.classList.toggle('boids-checkbox-on');
    collisions = false;
  }
}

// Mouse Seek
var mouseSeekInput = document.getElementById('mouse-seek');
mouseSeekInput.checked = true;
mouseSeekInput.onclick = function() {
  if ( !this.checked ) {
    this.checked = false;
    mouseSeekMobile.dataset.checked = false;
    mouseSeekMobile.classList.toggle('boids-checkbox-on');
    mouseSeek = false;
  } else {
    this.checked = true;
    mouseSeekMobile.dataset.checked = true;
    mouseSeekMobile.classList.toggle('boids-checkbox-on');
    mouseSeek = true;
  }
}
var mouseSeekMobile = document.getElementById('mouse-seek-mobile');
mouseSeekMobile.dataset.checked = true;
mouseSeekMobile.onclick = function() {
  if ( this.dataset.checked == 'false') {
    this.dataset.checked = true;
    mouseSeekInput.checked = true;
    this.classList.toggle('boids-checkbox-on');
    mouseSeek = true;
  } else {
    this.dataset.checked = false;
    mouseSeekInput.checked = false;
    this.classList.toggle('boids-checkbox-on');
    mouseSeek = false;
  }
}

// Introversion
var introversionControlContainer = document.getElementById('introversion-control-container');
var introversionInput = document.getElementById('introversion');
introversionInput.onchange = function() {
  introversion = this.value / 10;
  updateIntroversion(introversion);
}
var introversionMobile = document.getElementById('introversion-mobile');
introversionMobile.onclick = function() {
  document.getElementById('mobile-boids-controls').style.display = 'none';
  introversionControlContainer.classList.toggle('show');
}
function updateIntroversion(value) {
  for (var i=0; i<boids.length; i++) {
    boids[i].introversion = value * boids[i].introversionCoefficient;
  }
}

// Speed
var speedControlContainer = document.getElementById('speed-control-container');
var speedInput = document.getElementById('speed');
speedInput.onchange = function() {
  quickness = this.value / 10 + .5;
  updateQuickness(quickness);
}
var speedMobile = document.getElementById('speed-mobile');
speedMobile.onclick = function() {
  document.getElementById('mobile-boids-controls').style.display = 'none';
  speedControlContainer.classList.toggle('show');
}
function updateQuickness(value) {
  for (var i=0; i<boids.length; i++) {
    boids[i].quickness = value * boids[i].quicknessCoefficient;
    boids[i].maxSpeed = speedIndex * boids[i].quickness;
  }
}

// Racism
var racismControlContainer = document.getElementById('racism-control-container');
var racismInput = document.getElementById('racism');
racismInput.onchange = function() {
  racism = this.value / 5;
  updateRacism(racism);
}
var racismMobile = document.getElementById('racism-mobile');
racismMobile.onclick = function() {
  document.getElementById('mobile-boids-controls').style.display = 'none';
  racismControlContainer.classList.toggle('show');
}
function updateRacism(value) {
  for (var i=0; i<boids.length; i++) {
    boids[i].racism = value * boids[i].racismCoefficient;
  }
}

// Diversity
var diversityControlContainer = document.getElementById('diversity-control-container');
var diversityInput = document.getElementById('diversity');
diversityInput.onchange = function() {
  diversity = this.value;
  updateDiversity(diversity);
}
var diversityMobile = document.getElementById('diversity-mobile');
diversityMobile.onclick = function() {
  document.getElementById('mobile-boids-controls').style.display = 'none';
  diversityControlContainer.classList.toggle('show');
}
function updateDiversity(value) {
  for (var i=0; i<boids.length; i++) {
    boids[i].color = colors[ i % value ];
  }
}

/*---- end Inputs ----*/


```

可以注意到，鸟群是一个个的小球。我希望它们有粘性气泡效果
现在，请仔细思考后，输出需要修改部分的代码

todo: 
1. 球大小随界面变化（界面缩放<100% 球变大 反之变小）
2. 球的实际传送边框需要更远一些（或改为球半径超过边缘后再传送） done
3. 边界扩大后 球的流动性变差了（更少穿墙） 怎么解决
4. 和岩浆灯的区别是 不会融合（岩浆灯怎么离散？）可以考虑简单增大同色引力 
5. 增强动感 鼠标距离较远时增加移速

在boids_lavalamp中 进行修改 内有一个基于boids算法写的类岩浆灯前端 我希望修改之 当页面缩放值不同时默认球大小应该不同 以保证球在不同缩放值启动时大小在视觉上都类似

####
在一个html文件中完成该任务 写在dynamic_sidebar/index.html里 
该页面需要有左右两列细长的侧边栏 包含一列icon 作为按钮 按钮只有一个能被按下 按下时 出现一个侧边栏 里面先暂时放一系列方框 写一些文字
页面中间是一个大方框 写一些文字 

两侧侧边栏应当可以像pycharm软件那样 可以拖动来伸缩大小
侧边栏和页面主体中的内容应当是响应式的 适应大小


现在，请仔细思考后，拆分任务，逐步认真完成

####
做点修改:
1. 取消icon选中以收起侧边栏
2. 侧边栏内部的元素设置一个最小宽度 在此最小宽度之下 元素不再变小 而是被侧边栏的边框遮盖
现在，请仔细思考后，拆分任务，逐步认真完成

修改:
1. 取消icon选中以收起侧边栏 你刚刚没有实现 现在再次点击icon时根本就不会取消选中了 
2. 侧边栏的最小宽度 再调低一点 
现在，请仔细思考后，拆分任务，逐步认真完成

bug: 拖动侧边栏大小之后 无法收起了

####
在dynamic_sidebar/index.html里 存在bug 
要求: 在网页实现一个类似pycharm的效果: 
1. 两侧有icon按钮 单击出现对应侧边栏 再次单击收起 (可以直接出现消失 无需动画)
2. 两侧各只能生效一个按钮 按下一个按钮时 该侧其他按钮取消按下
3. 侧边栏宽度应当可以拉动调整 并保存下来 下次再点开时仍是该宽度

目前的bug是: 
1. 侧边栏出现消失时有我不需要的动画
2. 刚进入时一切正常 但是拖动侧边栏大小之后 单击按钮就无法收起侧边栏了

现在，请仔细思考，拆分任务，逐步认真修复以上提到的BUG