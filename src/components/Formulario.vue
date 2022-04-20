<template>
  <div class="box formulario">
    <div class="columns">
      <div
        class="column is-8"
        role="form"
        aria-label="Form for creating a new task"
      >
        <input
          type="text"
          class="input"
          placeholder="Which task do you want to start?"
          v-model="descricao"
        />
        <div class="column">
          <TempoRizador @aoTemporizadorFinalizado="finalizarTarefa" />
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import TempoRizador from "../components/Temporizador.vue";

export default defineComponent({
  name: "MyFormulario",
  emits: ["aoSalvarTarefa"],
  components: {
    TempoRizador,
  },
  data() {
    return {
      descricao: "",
    };
  },
  methods: {
    finalizarTarefa(tempoDecorrido: number): void {
      this.$emit("aoSalvarTarefa", {
        duracaoEmSegundos: tempoDecorrido,
        descricao: this.descricao,
      });
      this.descricao = "";
    },
  },
});
</script>

<style>
.formulario {
  color: var(--text-primary);
  background: var(--bg-primary);
}
</style>
