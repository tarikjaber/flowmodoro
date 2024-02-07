<script lang="ts">
  import Counter from './lib/Counter.svelte'
  
  let working = true

  let time: number = 0; 
  let startTime = new Date();
  let countDownTime: number;
  Notification.requestPermission()
  
  function breakOver() {
    new Notification("Break is over!")
  }
  
  function updateTime() {
    let currentTime = new Date();
    if (working) {
      let elapsedSeconds = Math.floor((+currentTime - +startTime) / 1000);
      time = elapsedSeconds;
    } else {
      let endTime = Math.floor(+startTime / 1000) + countDownTime;
      let timeLeft = endTime - Math.floor(+currentTime / 1000);

      time = timeLeft;
      if (timeLeft <= 0) {
        breakOver()
        startTime = new Date();
        working = true
      }
    }
  }
  
  setInterval(updateTime, 1000);

  function takeBreak() {
    working = false
    countDownTime = Math.floor(time / 5)
    time = countDownTime;
    startTime = new Date()
    // In case there is no time for a break
    if (countDownTime === 0) {
      working = true
    }
  }
  
  function formatTime(time: number) {
    let hours = Math.floor(time / 3600)
    let hourString = String(hours)
    let minutes = Math.floor(time / 60) - hours * 60
    let minuteString = String(minutes).padStart(2, '0')
    let seconds = time % 60
    let secondString = String(seconds).padStart(2, '0')
    
    if (hours > 0) {
     return hourString + ":" + minuteString + ":" + secondString 
    } else {
     return minuteString + ":" + secondString 
    }
  }
</script>


<svelte:head>
  <title>{working ? "Work" : "Break"}</title>
</svelte:head>
<Counter working={working}/>
<h1>
  {formatTime(time)}
</h1>
{#if working}
  <button on:click={takeBreak}>
    Break
  </button>
{/if}

<style>
  button {
    background-color: #FF8911;
  }
</style>

