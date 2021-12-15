<template>
  <button @click="handleClick">
    {{ note }}
  </button>
</template>

<script>
// TODO: why does the key bound note play louder/more velocity than the button clicks?
// add a remove event listner to the window element

import { Synth, Delay } from 'tone';
import dictionary from '@/lib/noteDictionary';
export default {
  props: {
    octave: Number,
    note: String,
  },
  data() {
    return {
      dictionary,
      oct: this.octave,
    };
  },
  mounted() {
    window.addEventListener('keyup', (e) => {
      if (e.key === 'k') {
        this.oct = this.octave + 1;
      } else {
        this.oct = this.octave;
      }
      this.handleKey(e);
    });
  },

  created() {
    this.synth = new Synth().toDestination();
    this.synth.volume.value = -3;
    this.delay = new Delay(0.4, 8).toDestination();
    this.synth.connect(this.delay);
  },
  computed: {
    noteAndOctave() {
      return this.dictionary[this.note].toUpperCase() + this.octave;
    },
  },
  methods: {
    handleClick() {
      this.synth.triggerAttackRelease(
        this.note.toUpperCase() + this.octave,
        '8n'
      );
    },
    handleKey(e) {
      if (this.dictionary[e.key]) {
        this.synth.triggerAttackRelease(
          this.dictionary[e.key].toUpperCase() + this.oct,
          '8n'
        );
      }
    }, //
  },
};
</script>

<style></style>
