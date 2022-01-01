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
  emits: ['setCurrentNote'],
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
      const note = this.dictionary[e.key]?.toUpperCase();
      //TODO -- add dynamic octave selection
      if (e.key === 'k') {
        this.octave = 5;
      } else {
        this.octave = 4;
      }
      this.$emit('setCurrentNote', note);
      if (this.dictionary[e.key]) {
        this.synth.triggerAttackRelease(note + this.octave, '8n');
      }
      return;
    },
    handleClick(note) {
      this.$emit('setCurrentNote', note);
      this.synth.triggerAttackRelease(note + this.octave, '8n');
    },
  },
};
</script>

<style></style>
