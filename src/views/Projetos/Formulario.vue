<template>
  <section>
    <form @submit.prevent="salvar">
      <div class="field">
        <label for="nomeDoProjeto" class="label display">Nome do Projeto</label>
        <input
          type="text"
          v-model="nomeDoProjeto"
          class="input"
          id="nomeDoProjeto"
        />
      </div>
      <div class="field">
        <button class="button mb-3" type="submit">Salvar</button>
      </div>
    </form>
  </section>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import { useStore } from "@/store/index";
// import { TipoNotificacao } from "@/interfaces/INotificacao";
// import { notificacaoMixin } from "../../mixins/notificar";
import useNotificador from "@/hooks/notificador";
import { TipoNotificacao } from "@/interfaces/INotificacao";
import { ALTERAR_PROJETO, CADASTRAR_PROJETO } from "@/store/tipo-acoes";
export default defineComponent({
  // eslint-disable-next-line vue/multi-word-component-names
  name: "Formulario",
  props: {
    id: {
      type: String,
    },
  },
  mounted() {
    if (this.id) {
      const projeto = this.store.state.projetos.find(
        (proj) => proj.id === this.id
      );
      this.nomeDoProjeto = projeto?.name || "";
    }
  },
  data() {
    return {
      nomeDoProjeto: "",
    };
  },
  // mixins: [notificacaoMixin],
  methods: {
    salvar() {
      if (this.id) {
        this.store
          .dispatch(ALTERAR_PROJETO, {
            id: this.id,
            name: this.nomeDoProjeto,
          })
          .then(() => {
            this.msgSucesso();
          });
      } else {
        this.store.dispatch(CADASTRAR_PROJETO, this.nomeDoProjeto).then(() => {
          this.msgSucesso();
        });
      }
    },
    msgSucesso() {
      this.nomeDoProjeto = "";
      this.notificar(
        TipoNotificacao.SUCESSO,
        "Excelente",
        "Foi salvo com sucesso"
      );
      this.$router.push("/projetos");
    },
  },
  setup() {
    const store = useStore();

    const { notificar } = useNotificador();

    return {
      store,
      notificar,
    };
  },
});
</script>
<style scoped>
.display {
  color: var(--text-primary);
}
</style>
