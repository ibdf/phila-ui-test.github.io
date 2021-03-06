<template>
  <div class="input-wrap input-checkbox">
    <label 
      :for="id"
      class="checkbox"
    >
      <input
        :id="id"
        v-bind="$attrs"
        :value="value"
        :checked="shouldBeChecked"
        :aria-checked="shouldBeChecked"
        :aria-labelledby="id"
        :aria-required="$attrs.required"
        type="checkbox"
        @change="updateInput"
        @on="$listeners"
      >
      <slot />
    </label>
  </div>
</template>
<script>
export default {
  name: 'InputCheckbox',
  inheritAttrs: false,
  model: {
    prop: 'modelValue',
    event: 'change',
  },
  props: {
    id: {
      type: String,
      default: () => `cb_${Math.random().toString(12).substring(2, 8)}`,
    },
    value: {
      type: [ String, Number, Boolean ],
      default: '',
    },

    // This prop represents the v-model value
    modelValue: {
      type: [ String, Number, Boolean, Array ],
      default: false,
    },

    /**
     * true-value and false-value are properties from vuejs checkboxes
     * used when you want to assign a value to the v-model when checked or unchecked
     * like 'yes' or 'no'
     */
    trueValue: {
      type: [ String, Number, Boolean ],
      default: true,
    },
    falseValue: {
      type: [ String, Number, Boolean ],
      default: false,
    },
  },
  computed: {
    shouldBeChecked() {
      if (this.modelValue) {
        if (this.modelValue instanceof Array) {
          return this.modelValue.includes(this.value);
        }

        return this.modelValue === this.trueValue
          || this.modelValue === this.value;
      }

      return this.$attrs.checked;
    },
  },
  methods: {
    updateInput(event) {
      let isChecked = event.target.checked;

      if (this.modelValue instanceof Array) {
        let newValue = [ ...this.modelValue ];

        if (isChecked) {
          newValue.push(this.value);
        } else {
          newValue.splice(newValue.indexOf(this.value), 1);
        }

        this.$emit('change', newValue);
      } else {
        if (this.value) {
          this.$emit('change', isChecked ? this.value : this.falseValue);
        } else {
          this.$emit('change', isChecked ? this.value : this.falseValue);
        }
      }
    },
  },
};
</script>
<style lang="scss">
  @import '../styles/InputCheckbox.scss';
</style>

