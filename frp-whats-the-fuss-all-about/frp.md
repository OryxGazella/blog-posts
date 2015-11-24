### Introduction ###

FRP, aka *functional reactive programming*, here is my understanding of what's going on in this space.

Let's start with something sufficiently simple. A game of tic-tac-toe. The reader may feel that this is something easy
to create in JavaScript. So let's start off with a single tile and build things up from there.

### Resources ###
If you'd like to gain a
much deeper understaning of the topic, I'm going to direct you away from this post and highlight the following resources.

#### Videos ####

* [1 - Bodi Stokke - Post FRP Frontend Programming](https://www.youtube.com/watch?v=X5YBsy6PaDw)
* [2 - André Staltz - Introduction to Reactive Programming](https://egghead.io/series/introduction-to-reactive-programming)
There's a very detailed gist that accompanies this series that I can highly recommend.

#### Books ####

<style type="text/css">
  .tile {
    position: relative;
    width: 30%;
    height: 0;
    padding-bottom: 32%;
    background-color: silver;
    margin: 1px;
  }

  #game {
    max-width: 70%;
    margin: auto;
  }

  @media screen and (min-width: 64em) {
    #game {
      max-width: 40%;
      margin: auto;
    }
  }

  .center-aligned {
    text-align: center;
  }

  .cross {
    background-image: url("/assets/cross.svg");
    background-size: 73%;
    background-repeat: no-repeat;
    background-position-x: center;
    background-position-y: center;
  }

  .circle {
    background-image: url("/assets/circle.svg");
    background-size: 73%;
    background-repeat: no-repeat;
    background-position-x: center;
    background-position-y: center;
  }
</style>

```css
.tile {
    position: relative;
    width: 30%;
    height: 0;
    padding-bottom: 32%;
    background-color: #C0B177;
    margin: 1px;
  }
```

```html
<div class="pure-g">
  <div class="pure-u-1-3 tile"></div>
</div>
```



<script src="https://cdnjs.cloudflare.com/ajax/libs/rxjs/4.0.7/rx.all.compat.min.js"></script>

<div id="coordinates" class="center-aligned"></div>

<div id="game">
  <div class="pure-g">
    <div id="oneOne" class="pure-u-1-3 tile"></div>
    <div class="pure-u-1-3 tile"></div>
    <div class="pure-u-1-3 tile"></div>
  </div>

  <div class="pure-g">
    <div class="pure-u-1-3 tile"></div>
    <div class="pure-u-1-3 tile"></div>
    <div class="pure-u-1-3 tile"></div>
  </div>

  <div class="pure-g">
    <div class="pure-u-1-3 tile"></div>
    <div class="pure-u-1-3 tile"></div>
    <div class="pure-u-1-3 tile"></div>
  </div>

  <script type="text/javascript">
    "use strict";

    var coords = document.getElementById('coordinates');
    var mouseMoveStream = Rx.Observable.fromEvent(document, 'mousemove');
    var subscription = mouseMoveStream.subscribe(function (e) {
      coords.innerHTML = e.clientX + ', ' + e.clientY;
    });

    var mouseClickStream = Rx.Observable.fromEvent(document.getElementById('game'), 'click');

    mouseClickStream.subscribe(function (e) {
      if(!e.target.className.match(/tile/)) return;
      var newIcon = e.target.className.match(/.*cross.*/) ? "circle" : "cross";
      e.target.className = e.target.className.replace(/(cross|circle)/, "");
      e.target.className += ' ' + newIcon;
    });
  </script>
</div>