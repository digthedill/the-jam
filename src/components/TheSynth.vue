<template>
  <button @click="handleClick">C L I C K</button>
</template>

<script>
import { Synth } from "tone";
import dictionary from "@/lib/noteDictionary";
export default {
  props: {
    octave: Number,
  },
  data() {
    return {
      dictionary,
      oct: this.octave,
    };
  },
  mounted() {
    window.addEventListener("keyup", (e) => {
      //this octave handling is not working...
      //not sure at the moment
      if (e.key === "k") {
        this.oct += 1;
      } else {
        this.oct = this.octave;
      }
      this.handleKey(e);
    });
  },

  created() {
    this.synth = new Synth().toDestination();
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
