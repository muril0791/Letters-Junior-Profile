<template>
  <div
    v-if="isPlayerInCell"
    class="pawn"
    :style="{ backgroundColor: player.color }"
  >
    <div class="pawn-label">{{ player.name }}</div>
  </div>
</template>

<script>
export default {
  props: {
    player: {
      type: Object,
      required: true,
    },
    cellIndex: {
      type: Number,
      required: true,
    },
  },
  computed: {
    isPlayerInCell() {
      return this.player.position === this.cellIndex;
    },
  },
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
