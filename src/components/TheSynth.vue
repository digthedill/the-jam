<template>
  <the-key
    v-for="(note, i) in notes"
    :key="`${note}${i}`"
    :note="note"
    :octave="octave"
    @click-note="handleClick"
  />
</template>

<script>
import { Synth } from 'tone';

import dictionary from '@/lib/noteDictionary';
import TheKey from '@/components/TheKey.vue';

export default {
  components: { TheKey },

  data() {
    return {
      dictionary,
      octave: 4,
      notes: Object.values(dictionary),
    };
  },
  mounted() {
    window.addEventListener('keyup', (e) => {
      this.handleKey(e);
    });
  },
  created() {
    this.synth = new Synth({
      volume: 0.5,
    }).toDestination();
  },
  methods: {
    handleKey(e) {
      console.log('anything?');
      if (this.dictionary[e.key]) {
        this.synth.triggerAttackRelease(
          this.dictionary[e.key].toUpperCase() + this.octave,
          '8n'
        );
      }
    },
    handleClick(n) {
      console.log('anything?');
      this.synth.triggerAttackRelease(n + this.octave, '8n');
    },
  },
};
</script>

<style></style>
