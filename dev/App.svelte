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

</script>

<style>
  .container{
    position: 'relative';
    display: 'flex';
    flex-direction: 'column';
    justify-content: 'center';
    align-items: 'center';
    width: '46vmax';
    height: '46vmax';
    margin: '2vmax 2vmax 0 0';
    background: grey;
  }
  button.reset{
    position: 'absolute';
    top: '1em';
    right: '1em';
    background: '#32bafa';
    color: '#fff';
    border: '0';
    border-radius: 4;
    font-size: '1em';
    padding: '0.7em 1.2em';
    cursor: 'pointer';
    outline: 'none';
  }

  button{
    background: '#121019';
    color: '#fff';
    padding: '1.5rem 3rem';
    border: '0';
    border-radius: 5;
    cursor: 'pointer';
    font-size: '1.2em';
  }
</style>

<div class="container" >
    <button class="reset" on:click={onToggle}>Reset</button>
  <ParticleEffectButton {hidden} onComplete={onAnimationComplete}>
    <button on:click={onToggle}>Whooh</button>
  </ParticleEffectButton>
</div>

