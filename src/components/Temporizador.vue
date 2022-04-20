<template>
  <div class="is-flex is-align-items-center is-justify-content-space-between">
    <CronoMetro :timeInSeconds="timeInSeconds" />
    <button class="button" @click="init()" :disabled="cronometroRodando">
      <span class="icon">
        <i class="fas fa-play"></i>
      </span>
      <span>play</span>
    </button>
    <button class="button" @click="finish()" :disabled="!cronometroRodando">
      <span class="icon">
        <i class="fas fa-stop"></i>
      </span>
      <span>stop</span>
    </button>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import CronoMetro from "../components/Cronometro.vue";

export default defineComponent({
  name: "TempoRizador",
  emits: ["aoTemporizadorFinalizado"],
  components: {
    CronoMetro,
  },
  data() {
    return {
      timeInSeconds: 0,
      stopwatch: 0,
      cronometroRodando: false,
    };
  },
  methods: {
    init() {
      this.cronometroRodando = true;
      this.stopwatch = setInterval(() => {
        this.timeInSeconds++;
      }, 1000);
    },
    finish() {
      this.cronometroRodando = false;
      clearInterval(this.stopwatch);
      this.$emit("aoTemporizadorFinalizado", this.timeInSeconds);
      this.timeInSeconds = 0;
    },
  },
});
</script>

<style scoped></style>
