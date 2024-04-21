<template>
  <div class="actions">
    <h2>Уровень: {{ level }}</h2>

    <button data-action="start" class="actions__button" @click="onStart" style="margin-top: 10px" :disabled="disabled">
      Начать
    </button>

    <div class="actions__settings">
      <div v-for="item in settings">
        <input type="radio" :value="item.value" v-model="settingsValue" class="pointed" />
        <label
          class="actions__settings-label pointed"
          @click="() => (settingsValue = item.value)"
          >{{ item.title }}</label
        >
      </div>
    </div>

    <p class="actions__message" style="margin-top: 10px">{{ userMessage }}</p>
  </div>
</template>

<script>
import { SETTINGS_INTERVAL } from '@/const/appSettings.js'

export default {
  props: {
    level: {
      type: Number,
      default: 0
    },
    userMessage: String,
    value: Number,
    disabled: Boolean
  },

  data() {
    return {
      settings: [
        { title: 'Легкий', value: SETTINGS_INTERVAL.EASY },
        { title: 'Средний', value: SETTINGS_INTERVAL.MEDIUM },
        { title: 'Сложный', value: SETTINGS_INTERVAL.HARD }
      ]
    }
  },

  computed: {
    settingsValue: {
      get() {
        return this.value
      },
      set(value) {
        this.$emit('input', value)
      }
    }
  },

  methods: {
    onStart() {
      this.$emit('onStart')
    }
  }
}
</script>

<style lang="scss" scoped>
.pointed {
  cursor: pointer;
}

.actions {
  padding: 0 30px;
  min-height: 300px;
}

.actions__button {
  width: 5em;
  box-sizing: border-box;
  font-size: 1.4em;
  -webkit-border-radius: 10px 10px 10px 10px;
  border-radius: 10px 10px 10px 10px;
  background: #6dabe8;
  border: none;
  padding: 0.3em 0.6em;

  &:hover {
    background: #78bcff;
    cursor: pointer;
  }
}

.actions__message {
  width: 200px;
}

.actions__settings {
  margin-top: 30px;
}

.actions__settings-label {
  margin-left: 10px;
}
</style>
