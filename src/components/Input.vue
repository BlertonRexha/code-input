<template>
  <input
    v-model="input_content"
    v-bind="$attrs"
    ref="input"
    type="text"
    maxlength="1"
    @input="handleInput"
    @keyup.delete="handleDelete"
    @keyup.left="handleArrow"
    @keyup.right="handleArrow"
    @paste="handlePaste"
  >
</template>

<script>
export default {
  name: 'AtomInput',
  props: {
    value: { required: true },
    index: { required: true },
  },
  data() {
    return {
      input_content: this.value
    };
  },
  methods: {
     handleInput(e) {
      this.$emit('input', this.input_content)
      if(e.data) this.$emit('inputChange', e)
    },
    handleDelete(e) {
      this.$emit('inputChange', e)
    },
    handleTab() {
      this.$emit('inputChange', {key: 'tab', data: 'next' })
    },
    handlePaste(e) {
      this.$emit('inputPaste', e.clipboardData.getData('text'))
      e.preventDefault()
    },
    handleArrow(e) {
      this.$emit('inputArrowChange', e)
    },
    focus() {
      this.$refs.input.focus();
    }
  }
}
</script>

<style lang="scss" scoped>
  input {
    border: none;
    border-bottom: 2px solid #ccc;
    padding: 5px;
    width: 20px;
    text-align: center;
    font-size: 1.1rem;
    &:focus{
      outline: none;
    }
  }
</style>