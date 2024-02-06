<script lang="ts">
  export let working: boolean
  let count: number = 0
  setInterval(() => {
    count += 1;
  }, 1000)
</script>

<h1>
  {working ? "Work" : "Break"}
</h1>
