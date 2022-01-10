<template>
  <button
    @click="handleClick"
    class="key"
    :class="isSharp ? 'black-key' : 'white-key'"
  >
    <div class="active-state"></div>
  </button>
</template>

<script>
/**TODO:
1) Upper octave doesn't change dynamically 

2) Grab key press state for ui

*/

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
  computed: {
    isSharp() {
      return this.note.includes('#');
    },
  },
};
</script>

<style lang="scss" scoped>
.key {
  margin-right: 16px;
  outline: none;
  border: none;
  display: flex;
  justify-content: center;
  align-items: flex-end;
  padding-bottom: 5px;
}
.white-key {
  width: 20px;
  height: 100px;
  background: $white;
  box-shadow: -1px -3px 4px rgba(255, 255, 255, 0.4),
    1px 3px 4px rgba(0, 0, 0, 0.25);
  border-radius: 100px;
}
.black-key {
  width: 15px;
  height: 60px;
  background: $black;
  opacity: 0.6;
  box-shadow: -1px -3px 4px rgba(255, 255, 255, 0.4),
    1px 3px 4px rgba(0, 0, 0, 0.25);
  border-radius: 100px;
  color: $white;
}

.active-state {
  //box-shadow
  background: #45ffff;
  border-radius: 100%;
  width: 8px;
  height: 8px;
  box-shadow: 0px 0px 5px #00dddd99;
}
</style>