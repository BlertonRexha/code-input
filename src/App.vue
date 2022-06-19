<template>
  <div id="app">
    <div class="input-wrapper" :key="digits">
      <input-code v-model="input_data" :digits="digits" />
    </div>
    <div class="description">
      <span> Set your code length! </span>
      <input v-model="digits" :min="2" :max="10" type="number">
      <p class="usage-code">
        Usage <pre>{{ usageMethod() }}</pre>
      </p>
    </div>
  </div>
</template>

<script>
import InputCode from './components/InputsWrapper.vue'

export default {
  name: 'ConfirmCodeInput',
  components: { InputCode },
  data() {
    return {
      input_data: null,
      digits: 6,
    };
  },
  methods: {
    usageMethod() {
      return `<input-code v-model="code" :digits="codeLength" />`
    }
  }
}
</script>

<style lang="scss" scoped>
  #app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    .input-wrapper {
      display: flex;
      justify-content: center;
      padding: 3em;
    }
    .description {
      text-align: center ;
      input {
        text-align: center ;
        padding: 7px 15px;
        &::-webkit-inner-spin-button {
          -webkit-appearance: none;
          margin: 0;
        }
         &:focus{
          outline: none;
        }
      }
      .usage-code {
        pre {
          background-color: #d4d4d4;
          display: flex;
          justify-content: center;
          padding: 7px 10px;
          width: max-content;
          margin: auto;
          border-radius: 3px;
        }
      }
    }
  }
</style>
