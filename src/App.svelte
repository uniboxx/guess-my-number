<script>
  import { onMount } from 'svelte';
  import Button from './lib/Button.svelte';

  onMount(resetGame);

  let secretNumber,
    secretNumberText,
    secretNumberElement,
    guessNumber,
    msg,
    score,
    highscore,
    win = false;

  //^ RESET FUNCTION

  function resetGame(event) {
    localStorage.highscore = localStorage.highscore || 0;
    highscore = localStorage.highscore;
    window.document.body.style.backgroundColor = '#222';
    win = false;
    secretNumberText = '?';
    secretNumber = Math.ceil(Math.random() * 20);
    guessNumber = ``;
    msg = 'Start guessing...';
    score = 20;
  }

  //^ check button

  function checkNumber(event) {
    if (!guessNumber) return (msg = '⛔️ No number!');
    if (guessNumber !== secretNumber && score > 0) score--;
    if (score === 0) return (msg = `💥 You lost the game!`);
    if (guessNumber < secretNumber) msg = '📉 Too low';
    if (guessNumber > secretNumber) msg = '📈 Too high';
    if (guessNumber === secretNumber) {
      window.document.body.style.backgroundColor = '#60b347';
      win = true;
      secretNumberText = secretNumber;
      msg = '🎉 Correct Number';
      if (score > highscore) highscore = localStorage.highscore = score;
    }
  }
</script>

<header>
  <section class="top">
    <Button type="" class="again" label={'Again!'} on:click={resetGame} />
    <p class="between">(Between 1 and 20)</p>
  </section>
  <h1>Guess My Number!</h1>
  <div bind:this={secretNumberElement} class="number" class:win>
    {secretNumberText}
  </div>
</header>
<main>
  <section class="left">
    <input
      type="number"
      class="guess"
      min="1"
      max="20"
      bind:value={guessNumber}
    />
    <Button
      type={'number'}
      class={'check'}
      label={'Check!'}
      on:click={checkNumber}
    />
  </section>
  <section class="right">
    <p class="message">{msg}</p>
    <p class="label-score">💯 Score: <span class="score">{score}</span></p>
    <p class="label-highscore">
      🥇 Highscore: <span class="highscore">{highscore}</span>
    </p>
  </section>
</main>

<style lang="stylus">
  header
    display flex
    flex-direction column
    // align-items center
    gap 1rem
    height 35vh
    border-bottom 7px solid #eee
    .top
      display flex
      flex-direction column
      justify-content start
      @media screen and (min-width 640px)
        margin 2rem
        flex-direction row
        justify-content space-between
      
        
    .between
      font-size 1.4rem
      text-align center
      margin-top .5rem
      @media screen and (min-width 640px)
        width auto

    h1
      font-size 1.9rem
      text-align center
      min-height 5rem
      padding 2rem 0
      @media screen and (min-width 640px)
        font-size 4rem
        margin 1rem
        
    
    .number
      align-self center
      background #eee
      color #333
      font-size 3rem
      padding 1rem 0rem
      text-align center
      border-radius 5px
      transform translateY(50%)
      transition all 1s ease
      @media screen and (min-width 640px)
        padding 2rem 0
        font-size 6rem
      &:not(.win)
        width 6rem
        @media screen and (min-width 640px)
          width 15rem
      &.win
        width 100%
        padding 2rem 0
        z-index 1
        
      
  main
    height 65vh
    color #eee
    display flex
    flex-direction column
    align-items center
    justify-content space-evenly
    gap 2rem
    @media screen and (min-width 640px)
      flex-direction row
  .left,.right
    width 100%
    display flex     
    
  .left
    flex-direction row
    justify-content space-around
    align-items center
    @media screen and (min-width 640px)
      justify-content center
      flex-direction column
  .right
    flex-direction column
    align-items center
    justify-content center
    font-size 2rem
   
  .guess
    background none
    border 4px solid #eee
    font-family inherit
    color inherit
    font-size 2.5rem
    padding 1rem
    width 11rem
    text-align center
    
    @media screen and (min-width 640px)
      width 25rem
      font-size 6rem
      margin-bottom 3rem
      padding 2.5rem

  .message
    margin-bottom 6rem
    height 2rem
    width 100%
    padding 0 5rem
    font-size 2rem
    @media screen and (min-width 640px)
      margin-bottom 8rem

  
  .label-score, .label-highscore
    margin-bottom 2rem
    width 100%
    padding 0 5rem
    font-size 1.5rem
    @media screen and (min-width 640px)
      font-size 2rem
    

</style>
