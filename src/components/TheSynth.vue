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
  <div class="keyboard-container">
    <the-key
      v-for="(note, i) in notes"
      :key="`${note}${i}`"
      :note="note"
      :octave="octave"
      @click-note="handleClick"
    />
  </div>
  <br>
  <label for="sequencer">enter sequencer</label>
  <input
    type="checkbox"
    name="sequencer"
    id="sequencer"
    v-model="showSequencer"
  >

  <div v-if="showSequencer">
    <h3>The Sequencer</h3>
    <p
      v-for="(note, i) in sequenceNotes"
      :key="`${renderNote(note)}${i}`"
    >
      {{note}}
    </p>
    <button @click="playSequence">Play</button>
    <button @click="stopSequence">Stop</button>
    <button @click="clearSequence">Clear</button>
  </div>

</template>

<script>
import { Sequence, Synth, Transport } from 'tone';

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
      showSequencer: false,
      oscillatorType: ['amsine', 'amsquare', 'amsawtooth', 'amtriangle'],
      oscillator: 'amsine',
      sequenceNotes: [],
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
  computed: {
    renderNote(n) {
      if (n === null) {
        return 'REST';
      } else return n;
    },
  },
  methods: {
    playSequence() {
      this.buildSequencer();
      Transport.start();
    },
    stopSequence() {
      Transport.stop();
    },
    clearSequence() {
      this.seq.clear();
      this.sequenceNotes = [];
    },
    handleKey(e) {
      Transport.start();
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
      if (
        this.showSequencer &&
        (Object.keys(this.dictionary).includes(e.key) || e.key === 'z')
      ) {
        if (e.key === 'z') {
          this.sequenceNotes.push(null);
        } else {
          this.sequenceNotes.push(note + this.octave);
        }
      }
      return;
    },
    handleClick(note) {
      this.$emit('setCurrentNote', note);
      this.synth.triggerAttackRelease(note + this.octave, '8n');
    },
    buildSequencer() {
      this.seq = new Sequence((time, note) => {
        this.synth.triggerAttackRelease(note, 0.1, time);
      }, this.sequenceNotes).start(0);
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
.keyboard-container {
  background: $background;
  display: flex;
  align-content: flex-start;
  justify-content: center;
  margin: 2rem;
  padding: 2rem;
  max-width: 554px;
}
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
