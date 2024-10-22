<script>
    import { onMount } from 'svelte';
    import anime from 'animejs';
    import raf from 'raf';

    export let hidden = false;
    export let duration = 1000;
    export let easing = 'easeInOutCubic';
    export let type = 'circle';
    export let style = 'fill';
    export let direction = 'left';
    export let canvasPadding = 150;

    let Wrapper, Canvas;

    export let size = () => Math.floor((Math.random() * 3) + 1);
    export let speed = () => rand(4);
    export let color = '#000';
    export let particlesAmountCoefficient = 3;
    export let oscillationCoefficient = 20;
    export let onBegin = () => {};
    export let onComplete = () => {};

    let particles = [];
    let ctx;

    let _raf = null;

  
    let status = hidden ? 'hidden' : 'normal';
    let progress = 0;
    let _progress;
  

    let _rect = {
      width: 0,
      height: 0
    };

    onMount(() => {

      const _status = status;

      if (_status === 'normal' && hidden) {
        status = 'hiding';
        startAnimation();
      } else if (_status === 'hidden' && !hidden) {
        status = 'showing';
        startAnimation();
      }
  
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

    function cycleStatus() {
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

    function addParticles(_progress_) {
      const _status = status
      const { width, height } = _rect;

      const delta = _status === 'hiding' ? _progress_ - _progress : _progress - _progress_
      const isHorizontal = isHorizontal()
      const progressValue = (isHorizontal ? width : height) * _progress_ + delta * (_status === 'hiding' ? 100 : 220);

      this._progress = _progress_

      let x = canvasPadding
      let y = canvasPadding

      if (isHorizontal) {
        x += direction === 'left' ? progressValue : width - progressValue
      } else {
        y += direction === 'top' ? progressValue : height - progressValue
      }

      let i = Math.floor(particlesAmountCoefficient * (delta * 100 + 1))
      if (i > 0) {
        while (i--) {
          addParticle({
            x: x + (isHorizontal ? 0 : width * Math.random()),
            y: y + (isHorizontal ? height * Math.random() : 0)
          })
        }
      }

      if (_raf) {
        _raf = raf(loop)
      }
    }

    function addParticle(opts){
      const _status = status

      const frames = duration * 60 / 1000
      const _speed = isFunc(speed) ? speed() : speed
      const _size = isFunc(size) ? size() : size

      _particles.push({
        startX: opts.x,
        startY: opts.y,
        x: _status === 'hiding' ? 0 : _speed * -frames,
        y: 0,
        angle: rand(360),
        counter: _status === 'hiding' ? 0 : frames,
        increase: Math.PI * 2 / 100,
        life: 0,
        death: _status === 'hiding' ? (frames - 20) + Math.random() * 40 : frames,
        speed: _speed,
        size: _size
      })
    }
    
    function updateParticles(){
      const _status = status
      for (let i = 0; i < _particles.length; i++) {
        const p = _particles[i]

        if (p.life > p.death) {
          _particles.splice(i, 1)
        } else {
          p.x += p.speed
          p.y = oscillationCoefficient * Math.sin(p.counter * p.increase)
          p.life++
          p.counter += (_status === 'hiding' ? 1 : -1)
        }
      }
    }

    function renderParticles(){
      const _status = status

      ctx.clearRect(0, 0, Canvas.width, Canvas.height)
      ctx.fillStyle = ctx.strokeStyle = color

      for (let i = 0; i < _particles.length; ++i) {
        const p = _particles[i]

        if (p.life < p.death) {
          ctx.translate(p.startX, p.startY)
          ctx.rotate(p.angle * Math.PI / 180)
          ctx.globalAlpha = (status === 'hiding') ? 1 - p.life / p.death : p.life / p.death
          ctx.beginPath()

          if (type === 'circle') {
            ctx.arc(p.x, p.y, p.size, 0, 2 * Math.PI)
          } else if (type === 'triangle') {
            ctx.moveTo(p.x, p.y)
            ctx.lineTo(p.x + p.size, p.y + p.size)
            ctx.lineTo(p.x + p.size, p.y - p.size)
          } else if (type === 'rectangle') {
            ctx.rect(p.x, p.y, p.size, p.size)
          }

          if (style === 'fill') {
            ctx.fill()
          } else if (style === 'stroke') {
            ctx.closePath()
            ctx.stroke()
          }

          ctx.globalAlpha = 1
          ctx.rotate(-p.angle * Math.PI / 180)
          ctx.translate(-p.startX, -p.startY)
        }
      }
    }

    function isHorizontal() {
      return direction === 'left' || direction === 'right';
    }
    
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