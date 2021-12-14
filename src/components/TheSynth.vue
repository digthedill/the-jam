<template>
  <button @click="handleClick">
    {{ note }}
  </button>
</template>

<script>
import { Synth, Delay } from "tone";
import dictionary from "@/lib/noteDictionary";
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
    window.addEventListener("keyup", (e) => {
      if (e.key === "k") {
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
      this.synth.triggerAttackRelease(this.noteAndOctave, "8n");
    },
    handleKey(e) {
      if (this.dictionary[e.key]) {
        this.synth.triggerAttackRelease(
          this.dictionary[e.key].toUpperCase() + this.oct,
          "8n"
        );
      }
    }, //
  },
};
</script>

<style></style>
