<script>
  import ParticleEffectButton from '../src/index.js';

    let hidden = false;
    let animating = false;

  let onToggle = () => {
    if (animating) return;
      hidden = !hidden,
      animating = true
   }

  let onAnimationComplete = () => {
      animating = false
  }

    let background ='linear-gradient(120deg,#7f54e6,#3F51B5)',
    text = 'Upload',
    buttonStyles = {
      background: '#f3f3f3',
      color: '#3c2e9e',
      padding: '1.8rem 4rem'
    },
    buttonOptions = {
      color: '#f3f3f3',
      type: 'triangle',
      easing: 'easeOutQuart',
      size: 6,
      particlesAmountCoefficient: 4,
      oscillationCoefficient: 2
    }

</script>

<style>
  .container{
    position: relative;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    width: 46vmax;
    height: 46vmax;
    margin: 2vmax 2vmax 0 0;
    background: grey;
  }
  button.reset{
    position: absolute;
    top: 1em;
    right: 1em;
    background: #32bafa;
    color: #fff;
    border: 0;
    border-radius: 4;
    font-size: 1em;
    padding: 0.7em 1.2em;
    cursor: pointer;
    outline: none;
  }

  button{
    color: #fff;
    border: 0;
    border-radius: 5;
    cursor: pointer;
    font-size: 1.2em;
    background: #f3f3f3;
    color: #3c2e9e;
    padding: 1.8rem 4rem;
  }
</style>

<div class="container" >
    <button class="reset" on:click={onToggle}>Reset</button>
  <ParticleEffectButton {hidden} onComplete={onAnimationComplete} {...buttonOptions}>
    <button on:click={onToggle}>{text}</button>
  </ParticleEffectButton>
</div>

