<template>
  <div class="pawn" :style="{ backgroundColor: color }">
    <div class="pawn-label">{{ playerName }}</div>
  </div>
</template>

<script>
export default {
  props: ['color', 'playerName']
}
</script>

<style>
.pawn {
  width: 100px;
  height: 100px;
  border-radius: 50%;
  position: relative;
  display: flex;
  align-items: center;
  justify-content: center;
}

.pawn-label {
  font-size: 14px;
  color: #fff;
}
</style>
