<template>
  <div class="container">
    <SoundsBlock />

    <ButtonsBlock :tiles="tiles" @handleClick="handleClick" :disabled="disabledButtons" />

    <ActionsBlock
      @onStart="onStart"
      :level="level"
      :userMessage="userMessage"
      v-model="delaySetting"
      @input="() => resetGame()"
      :disabled="disabledStart"
    />
  </div>
</template>

<script>
import SoundsBlock from '@/components/SoundsBlock.vue'
import ButtonsBlock from '@/components/ButtonsBlock.vue'
import ActionsBlock from '@/components/ActionsBlock.vue'

import { SETTINGS_INTERVAL } from '@/const/appSettings.js'

export default {
  components: {
    ButtonsBlock,
    ActionsBlock,
    SoundsBlock
  },

  data() {
    return {
      tiles: ['red', 'green', 'blue', 'yellow'],
      level: 0,
      sequence: [],
      humanSequence: [],

      userMessage: '',
      delaySetting: SETTINGS_INTERVAL.EASY,
      SHOW_TILE_TIME: 300, // мс
      disabledButtons: false,
      disabledStart: false
    }
  },

  methods: {
    onStart() {
      this.resetGame()
      this.disabledStart = true
      this.userMessage = 'Ожидайте... Очередь компьютера'
      this.nextRound()
    },

    resetGame(message) {
      this.disabledButtons = true
      this.disabledStart = false
      this.userMessage = message ?? ''
      this.level = 0
      this.sequence = []
      this.humanSequence = []
    },

    nextRound() {
      this.level += 1
      this.disabledButtons = true

      const nextSequence = [...this.sequence]
      nextSequence.push(this.nextStep())
      this.playRound(nextSequence)

      this.sequence = [...nextSequence]
      setTimeout(() => {
        this.humanTurn(this.level)
      }, this.level * this.delaySetting + 1000)
    },

    nextStep() {
      const random = this.tiles[Math.floor(Math.random() * this.tiles.length)]
      return random
    },

    activateTile(color) {
      const tile = document.querySelector(`[data-tile='${color}']`)
      const sound = document.querySelector(`[data-sound='${color}']`)

      tile.classList.add('activated')
      sound.play()

      setTimeout(() => {
        tile.classList.remove('activated')
      }, this.SHOW_TILE_TIME)
    },

    playRound(nextSequence) {
      nextSequence.forEach((color, index) => {
        setTimeout(() => {
          this.activateTile(color)
        }, (index + 1) * this.delaySetting)
      })
    },

    humanTurn(level) {
      this.disabledButtons = false
      this.userMessage = `Ваша очередь: eще ${level}`
    },

    handleClick(tile) {
      const index = this.humanSequence.push(tile) - 1
      const sound = document.querySelector(`[data-sound='${tile}']`)
      sound.play()

      const remainingTaps = this.sequence.length - this.humanSequence.length

      if (this.humanSequence[index] !== this.sequence[index]) {
        this.resetGame('Упс! Игра окончена, вы нажали неверную кнопку')
        return
      }

      if (this.humanSequence.length === this.sequence.length) {
        this.humanSequence = []
        this.userMessage = 'Успех! Продолжайте в том же духе!'
        setTimeout(() => {
          this.nextRound()
        }, 1000)
        return
      }

      this.userMessage = `Ваша очередь: eще ${remainingTaps}`
    }
  }
}
</script>

<style lang="scss" scoped>
.container {
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
}
</style>
