<template>
  <div class="box formulario">
    <div class="columns">
      <div
        class="column is-12"
        role="form"
        aria-label="Form for creating a new task"
      >
        <input
          type="text"
          class="input"
          placeholder="Which task do you want to start?"
          v-model="descricao"
        />
        <div class="column is-12">
          <div class="select">
            <select v-model="idProjeto">
              <option value="">Selecione o projeto</option>
              <option
                :value="projeto.id"
                v-for="projeto in projetos"
                :key="projeto.id"
              >
                {{ projeto.name }}
              </option>
            </select>
          </div>
        </div>
        <div class="column">
          <Temporizador @aoTemporizadorFinalizado="salvarTarefa" />
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { computed, defineComponent } from "vue";
import Temporizador from "../components/Temporizador.vue";
import { key } from "../store/index";
import IProjeto from "../interfaces/IProjeto";
import { NOTIFICAR } from "../store/tipo-mutacoes";
import { TipoNotificacao } from "@/interfaces/INotificacao";
import { useStore } from "@/store/index";
export default defineComponent({
  // eslint-disable-next-line vue/multi-word-component-names
  name: "Formulario",
  emits: ["aoSalvarTarefa"],
  components: {
    Temporizador,
  },
  data() {
    return {
      descricao: "",
      idProjeto: "",
    };
  },
  methods: {
    salvarTarefa(tempoDecorrido: number): void {
      this.$emit("aoSalvarTarefa", {
        duracaoEmSegundos: tempoDecorrido,
        descricao: this.descricao,
        projeto: this.projetos.find(
          (proj: IProjeto) => proj.id === this.idProjeto
        ),
      });
      this.descricao = "";

      const projeto = this.projetos.find((p) => p.id == this.idProjeto);
      if (!projeto) {
        this.store.commit(NOTIFICAR, {
          titulo: "Ops!",
          texto: "Selecione um projeto antes de finalizar a tarefa!",
          tipo: TipoNotificacao.FALHA,
        });
        return;
      }
    },
  },
  setup() {
    const store = useStore();
    return {
      projetos: computed(() => store.state.projetos),
      store,
    };
  },
});
</script>

<style>
.formulario {
  color: var(--text-primary);
  background: var(--bg-primary);
}
</style>
