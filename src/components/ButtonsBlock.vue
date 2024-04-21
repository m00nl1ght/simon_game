<template>
  <div class="buttons-container">
    <ul :class="{ unclickable: disabled }">
      <li
        v-for="tile in tiles"
        :key="tile"
        class="button"
        :class="tile"
        :data-tile="tile"
        @click="onClick(tile)"
      ></li>
    </ul>
  </div>
</template>

<script>
export default {
  props: {
    tiles: {
      type: Array,
      default: () => []
    },

    disabled: Boolean
  },

  methods: {
    onClick(val) {
      this.$emit('handleClick', val)
    }
  }
}
</script>

<style lang="scss" scoped>
ul {
  padding: 0;
}

.buttons-container {
  background: #fff;
  position: relative;
  width: 300px;
  height: 300px;
  box-shadow: 2px 1px 12px #aaa;
  border-radius: 150px;
}

.button {
  opacity: 0.6;
  height: 300px;
  -webkit-border-radius: 150px 150px 150px 150px;
  border-radius: 150px;
  position: absolute;

  &:hover {
    border: 2px solid black;
    cursor: pointer;
  }

  &:active {
    opacity: 1;
  }
}

.button.activated {
  opacity: 1;
}

.red {
  background: #ff5643;
  clip: rect(0px, 300px, 150px, 150px);
  width: 300px;
}

.blue {
  background: dodgerblue;
  clip: rect(0px, 150px, 150px, 0px);
  width: 300px;
}

.yellow {
  background: #feef33;
  clip: rect(150px, 150px, 300px, 0px);
  width: 300px;
}

.green {
  background: #bede15;
  clip: rect(150px, 300px, 300px, 150px);
  width: 300px;
}

.unclickable {
  pointer-events: none;
}
</style>
