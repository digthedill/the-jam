<template>

  <div class="envelope">
    <label for="oscillator">oscillator</label>
    <select
      name="oscillator"
      id="oscillator"
      v-model="oscillator"
      @change="buildSynth"
    >
      <option
        v-for="osc in oscillatorType"
        :key="osc"
        :value="osc"
      >
        {{osc}}
      </option>
    </select>

    <label for="attack">attack</label>

    <input
      type="range"
      name="attack"
      id="attack"
      min="0"
      max="100"
      v-model="envelope.attack"
      @change="buildSynth"
    >
    <label for="decay">decay</label>
    <input
      type="range"
      name="decay"
      id="decay"
      min="0"
      max="100"
      v-model="envelope.decay"
      @change="buildSynth"
    >
    <label for="sustain">sustain</label>
    <input
      type="range"
      name="sustain"
      id="sustain"
      min="0"
      max="100"
      v-model="envelope.sustain"
      @change="buildSynth"
    >
    <label for="release">release</label>
    <input
      type="range"
      name="release"
      id="release"
      min="0"
      max="100"
      v-model="envelope.release"
      @change="buildSynth"
    >

  </div>
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
      envelope: {
        attack: 10,
        decay: 20,
        sustain: 50,
        release: 80,
      },
      oscillatorType: ['amsine', 'amsquare', 'amsawtooth', 'amtriangle'],
      oscillator: 'amsine',
    };
  },
  mounted() {
    window.addEventListener('keyup', (e) => {
      this.handleKey(e);
    });
  },
  created() {
    this.buildSynth();
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
    buildSynth() {
      const envelope = Object.keys(this.envelope).reduce((acc, key) => {
        acc[key] = this.envelope[key] / 100;
        return acc;
      }, {});

      this.synth = new Synth({
        oscillator: {
          partialCount: 0,
          partials: [],
          phase: 0,
          type: this.oscillator,
          harmonicity: 0.5,
          modulationType: 'triangle',
        },
        envelope,
      }).toDestination();
    },
  },
};
</script>

<style scoped lang="scss">
.container {
  max-width: 600px;
  margin: auto;
}
.envelope {
  display: flex;
  flex-direction: column;
  & > * {
    width: 200px;
  }
}
</style>
