<script>

    let hidden = false;
    let duration = 1000;
    let easing = 'easeInOutCubic';
    let type = 'circle';
    let style = 'fill';
    let direction = 'left';
    let canvasPadding = 150;

    let Wrapper, Canvas;

    let size = () => Math.floor((Math.random() * 3) + 1);
    let speed = () => rand(4);
    let color = '#000';
    let particlesAmountCoefficient = 3;
    let oscillationCoefficient = 20;
    let onBegin = () => {};
    let onComplete = () => {};
    
 </script>


 <style>
  .particles {
    position: relative;
    display: inline-block;
  }

  .wrapper {
    position: relative;
    display: inline-block;
    overflow: hidden;
  }

  .content:focus,
  .content > *:focus {
    outline: none;
  }

  .canvas {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate3d(-50%, -50%, 0);
    pointer-events: none;
  }
 </style>

<div class='particles'>
  <div class="wrapper" style="" bind:this={Wrapper}>
    <div class="content" style="">
      <slot></slot>
    </div>
  </div>

  <canvas class="canvas" bind:this={Canvas} style="" />
</div>