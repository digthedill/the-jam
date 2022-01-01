<template>
  <button @click="handleClick">
    {{ note }}
  </button>
</template>

<script>
//octave doen't change for the last C
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