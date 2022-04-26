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
import { defineComponent, ref } from "vue";
import { useStore } from "@/store/index";
// import { TipoNotificacao } from "@/interfaces/INotificacao";
// import { notificacaoMixin } from "../../mixins/notificar";
import useNotificador from "@/hooks/notificador";
import { TipoNotificacao } from "@/interfaces/INotificacao";
import { ALTERAR_PROJETO, CADASTRAR_PROJETO } from "@/store/tipo-acoes";
import { useRouter } from "vue-router";
export default defineComponent({
  // eslint-disable-next-line vue/multi-word-component-names
  name: "Formulario",
  props: {
    id: {
      type: String,
    },
  },
  // mounted() {
  //   if (this.id) {
  //     const projeto = this.store.state.projeto.projetos.find(
  //       (proj) => proj.id == this.id
  //     );
  //     this.nomeDoProjeto = projeto?.name || "";
  //   }
  // },
  // data() {
  //   return {
  //     nomeDoProjeto: "",
  //   };
  // },
  // mixins: [notificacaoMixin],
  // methods: {

  // },
  setup(props) {
    const router = useRouter();

    const store = useStore();
    const { notificar } = useNotificador();

    const nomeDoProjeto = ref("");

    if (props.id) {
      const projeto = store.state.projeto.projetos.find(
        (proj) => proj.id == props.id
      );
      nomeDoProjeto.value = projeto?.name || "";
    }

    const msgSucesso = () => {
      nomeDoProjeto.value = "";
      notificar(TipoNotificacao.SUCESSO, "Excelente", "Foi salvo com sucesso");
      router.push("/projetos");
    };

    const salvar = () => {
      if (props.id) {
        store
          .dispatch(ALTERAR_PROJETO, {
            id: props.id,
            name: nomeDoProjeto.value,
          })
          .then(() => {
            msgSucesso();
          });
      } else {
        store.dispatch(CADASTRAR_PROJETO, nomeDoProjeto.value).then(() => {
          msgSucesso();
        });
      }
    };

    return {
      nomeDoProjeto,
      salvar,
    };
  },
});
</script>
<style scoped>
.display {
  color: var(--text-primary);
}
</style>
