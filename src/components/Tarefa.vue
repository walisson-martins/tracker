<template>
  <Box>
    <div class="columns clicavel" @click="tarefaClicado">
      <div class="column is-4">
        {{ tarefa.descricao || "Tarefa sem descrição" }}
      </div>
      <div class="column is-3">
        {{ tarefa.projeto?.name || "N/D " }}
      </div>
      <div class="column">
        <Cronometro :timeInSeconds="tarefa.duracaoEmSegundos" />
      </div>
    </div>
  </Box>
</template>

<script lang="ts">
import { defineComponent, PropType } from "vue";
import Cronometro from "../components/Cronometro.vue";
import ITarefa from "../interfaces/ITarefa";
import Box from "../components/Box.vue";

export default defineComponent({
  // eslint-disable-next-line vue/multi-word-component-names
  name: "Tarefa",
  emits: ["aoTarefaClicada"],
  components: {
    Cronometro,
    Box,
  },
  props: {
    tarefa: {
      type: Object as PropType<ITarefa>,
      required: true,
    },
  },
  methods: {
    tarefaClicado(): void {
      this.$emit("aoTarefaClicada", this.tarefa);
    },
  },
});
</script>
<style scoped>
.clicavel {
  cursor: pointer;
}
</style>
