<template>
  <main
    class="columns is-gapless is-multiline"
    :class="{ 'modo-escuro': modoEscuroAtivo }"
  >
    <div class="column is-one-quarter">
      <BarraLateral @aoTemaAlterado="trocarTema" />
    </div>
    <div class="column is-tree-quarter conteudo">
      <MyFormulario @aoSalvarTarefa="salvarTarefa" />
      <div class="lista">
        <TaRefa
          v-for="(tarefa, index) in tarefas"
          :key="index"
          :tarefa="tarefa"
        />
        <BOx v-if="listaEstaVazia"> Você não está produtivo hoje :( </BOx>
      </div>
    </div>
  </main>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import BarraLateral from "./components/BarraLateral.vue";
import MyFormulario from "./components/Formulario.vue";
import TaRefa from "./components/Tarefa.vue";
import ITarefa from "./interfaces/ITarefa";
import BOx from "./components/Box.vue";
export default defineComponent({
  name: "App",
  components: {
    BarraLateral,
    MyFormulario,
    TaRefa,
    BOx,
  },
  data() {
    return {
      tarefas: [] as ITarefa[],
      modoEscuroAtivo: false,
    };
  },
  computed: {
    listaEstaVazia(): boolean {
      return this.tarefas.length === 0;
    },
  },
  methods: {
    salvarTarefa(tarefa: ITarefa): void {
      this.tarefas.push(tarefa);
    },
    trocarTema(modoEscuroAtivo: boolean): void {
      this.modoEscuroAtivo = modoEscuroAtivo;
    },
  },
});
</script>

<style>
.lista {
  padding: 1.25rem;
}

main {
  --bg-primary: #fff;
  --text-primary: #000;
}

main.modo-escuro {
  --bg-primary: #2b2d42;
  --text-primary: #ddd;
}

.conteudo {
  background: var(--bg-primary);
}
</style>
