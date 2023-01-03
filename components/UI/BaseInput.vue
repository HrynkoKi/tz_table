<template>
  <div>
    <label>
      {{ label }}
      <input
        :name="name"
        v-model="model"
        type="text"
        class="input"
      />
    </label>
  </div>
</template>

<script>
export default {
  name: 'BaseInput',
  emits: ['input'],
  props: {
    label: {
      type: String,
      required: true,
    },
    value: {
      type: String,
      required: true,
    },
  },
  computed: {
    name() {
      return this.label.toLowerCase();
    },
    model: {
      get() {
        return this.value;
      },
      set(value) {
        this.$emit('input', value);
      },
    },
  },
}
</script>
<style lang="scss">
label {
  display: block;
}

.input {
  display: block;
  border: none;
  outline: 2px solid $main-color;
}
</style>