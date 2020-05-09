<template>
  <div class="container">
    <div class="player">
      <h4 class="player-name">You</h4>
      <div class="health-bar">
        <p class="healthRemaining">{{ monsterHealth }}</p>
        <div class="health-remaining" :style="{}"></div>
        <div class="health-empty"></div>
      </div>
    </div>
    <div class="player">
      <h4 class="player-name">MONSTER</h4>
      <div class="health-bar">
        <p class="healthRemaining">{{ monsterHealth }}</p>
        <div class="health-remaining" :style="{}"></div>
        <div class="health-empty"></div>
      </div>
    </div>
  </div>
</template>
<script>
  export default {};
</script>