<template>
  <div class="home">
    <h1>hello world</h1>
    <the-synth
      v-for="note in notes"
      :key="note"
      :note="note"
      :octave="4"
    >
    </the-synth>
  </div>
</template>

<script>
import TheSynth from '@/components/TheSynth.vue';
export default {
  components: { TheSynth },
  name: 'Home',
  data() {
    return {
      notes: ['a', 'b', 'c', 'd', 'e', 'f', 'g'],
      currentNote: null,
    };
  },
};
</script>
