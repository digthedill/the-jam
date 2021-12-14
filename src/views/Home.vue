<template>
  <div class="home">
    <h1>hello world</h1>
    <the-synth
      note="a"
      :octave="4"
    />
  </div>
</template>

<script>
import TheSynth from "@/components/TheSynth.vue";
export default {
  components: { TheSynth },
  name: "Home",
};
</script>
