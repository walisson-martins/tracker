<template>
  <BOx>
    <div class="columns">
      <div class="column is-7">
        {{ tarefa.descricao || "Tarefa sem descrição" }}
      </div>
      <div class="column">
        <CronoMetro :timeInSeconds="tarefa.duracaoEmSegundos" />
      </div>
    </div>
  </BOx>
</template>

<script lang="ts">
import { defineComponent, PropType } from "vue";
import CronoMetro from "../components/Cronometro.vue";
import ITarefa from "../interfaces/ITarefa";
import BOx from "../components/Box.vue";

export default defineComponent({
  name: "TaRefa",
  components: {
    CronoMetro,
    BOx,
  },
  props: {
    tarefa: {
      type: Object as PropType<ITarefa>,
      required: true,
    },
  },
});
</script>
