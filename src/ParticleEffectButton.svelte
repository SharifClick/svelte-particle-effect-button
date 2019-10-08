<script>

    let hidden = false;
    let duration = 1000;
    let easing = 'easeInOutCubic';
    let type = 'circle';
    let style = 'fill';
    let direction = 'left';
    let canvasPadding = 150;
    
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