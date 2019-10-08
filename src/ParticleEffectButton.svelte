<script>
    import { onMount } from 'svelte';
    import anime from 'animejs'

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

    let particles = [];
    let ctx;

  
    let status = hidden ? 'hidden' : 'normal';
    let progress = 0;
    let _progress;
  

    let _rect = {
      width: 0,
      height: 0
    };

    onMount(() => {
      
    });

    function rand(value) {
      return Math.random() * value - value / 2;
    }

    function isFunc(value) {
      return typeof value === 'function';
    }

    let startAnimation = () => {
      if (!Canvas || !Wrapper) return

      let _status = status
      let _particles = particles;

      if (_status === 'hiding') {
        this._progress = 0
      } else {
        this._progress = 1
      }


      _rect = Wrapper.getBoundingClientRect();
      Canvas.width = _rect.width + canvasPadding * 2;
      Canvas.height = _rect.height + canvasPadding * 2;
      ctx = Canvas.getContext('2d');

      anime({
        targets: { value: (_status === 'hiding') ? 0 : 100 },
        value: (_status === 'hiding') ? 100 : 0,
        duration: duration,
        easing: easing,
        begin: onBegin,
        update: (anim) => {
          const value = anim.animatables[0].target.value
          setTimeout(() => {
            progress = value;
          })

          if (duration) {
            addParticles(value / 100)
          }
        }
      })
    }

    cycleStatus() {
      const _status = status

      if (_status === 'normal') {
        status = 'hiding'; 
      } else if (_status === 'hidden') {
        status = 'showing'; 
      } else if (_status === 'hiding') {
        status = 'hidden';
      } else if (_status === 'showing') {
        status = 'normal';
      }
    }

    let loop = () => {
      updateParticles()
      renderParticles()

      if (_particles.length) {
        _raf = raf(loop)
      } else {
        _raf = null
        cycleStatus()
        onComplete()
      }
    }

    addParticles(opts){

    }
    updateParticles(){}
    renderParticles(){}
    
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