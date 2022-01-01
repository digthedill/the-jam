<template>
  <div class="home">
    <h1>Super Duper Synth</h1>
    <the-synth @set-current-note="setCurrentNote" />

  </div>
</template>

<script>
import TheSynth from '@/components/TheSynth.vue';
export default {
  components: { TheSynth },
  name: 'Home',
  data() {
    return {
      currentNote: null,
    };
  },
  methods: {
    setCurrentNote(note) {
      this.currentNote = note;
    },
  },
};
</script>
