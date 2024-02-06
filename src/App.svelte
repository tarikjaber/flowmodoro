<script lang="ts">
  import Counter from './lib/Counter.svelte'
  
  let working = true

  let time: number = 0; 
  let permission = Notification.requestPermission()
  
  function breakOver() {
    new Notification("Break is over!")
  }
  
  function updateTime() {
    if (working) {
      time += 1;
    } else {
      time -= 1;
      if (time === 0) {
        breakOver()
        working = true
      }
    }
  }
  
  setInterval(updateTime, 1000)

  function takeBreak() {
    working = false
    time = Math.floor(time / 5)
    // In case there is no time for a break
    if (time === 0) {
      working = true
    }
  }
  
  function formatTime(time: number) {
    let hours = Math.floor(time / 3600)
    let hourString = String(hours)
    let minutes = Math.floor(time / 60)
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

