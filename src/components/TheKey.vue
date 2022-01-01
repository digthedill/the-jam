<template>
  <button @click="handleClick">
    {{ note }}
  </button>
</template>

<script>
export default {
  props: {
    note: String,
    octave: Number,
  },
  emits: ['clickNote'],
  methods: {
    handleClick() {
      this.$emit('clickNote', this.note);
    },
  },
};
</script>

<style>
</style>