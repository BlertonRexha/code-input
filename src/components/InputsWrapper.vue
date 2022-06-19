<template>
  <div class="input-wrapper">
    <div v-for="(elem, index) in model" :key="index">
      <atom-input
        v-model="elem.value"
        v-bind="$attrs"
        :index="index"
        :key="refreshKey"
        :ref="`input_ref_${index}`"
        @click.native="handleChange(index)"
        @inputChange="handleChange(index, $event)"
        @inputPaste="handlePaste(index, $event)"
        @inputArrowChange="handleArrow(index, $event)"
      />
    </div>
  </div>
</template>

<script>
import AtomInput from './Input.vue'
export default {
  name: 'InputsWrapper',
  components: { AtomInput },
  props: {
    digits: {
      typeof: Number,
      default: 6,
    },
    value: {
      required: true,
    }
  },
  data() {
    return {
      model: [],
      focusIndex: 0,
      refreshKey: 1,
    };
  },
  beforeMount() {
    for (let index = 0; index < this.digits; index++) {
      this.model.push({ value: null })
    }
  },
  methods: {
    handlePaste(index, e = null) {
      if (e)
        for (let i = index; i < this.digits; i++) {
          if (e.charAt(i - index))
            this.model[i].value = e.charAt(i - index)
        }
      this.refreshKey++
      this.$emit('input', this.model.map(elem => elem.value).join(''))
    },
    handleChange(index, e = null) {
      if(e) {
        this.focusIndex += ![null, undefined].includes(e.data)
          ? (index + (e.key === 'tab' ? 0 : 1) === this.digits) ? 0 : +1 : (!index ? 0 : -1)
        this.$refs[`input_ref_${this.focusIndex}`][0].focus()
      } else {
        this.focusIndex = index
        this.$refs[`input_ref_${this.focusIndex}`][0].focus()
      }
      this.$emit('input', this.model.map(elem => elem.value).join(''))
    },
    handleArrow(index, e = null) {
      if (e) {
        this.focusIndex = e.code === 'ArrowRight' ? index < this.digits - 1 ? index + 1 : index : index > 0 ? index - 1 : index
        this.$refs[`input_ref_${this.focusIndex}`][0].focus()
      }
    },
  }
}
</script>
<style lang="scss" scoped>
.input-wrapper {
  display: flex;
  gap: 10px;
}
</style>
