<template>
    <section>
        <form @submit.prevent="salvar">
            <div class="field">
                <label for="nomeDoProjeto" class="label">
                    Nome do projeto
                </label>
                <input type="text" class="input" v-model="nomeDoProjeto" id="nomeDoProjeto">
            </div>
        
            <div class="field">
                <button class="button" type="submit">
                    Salvar
                </button>
            </div>
        </form>
    </section>
</template>

<script lang="ts">
    import { defineComponent } from 'vue';
    import { useStore } from '@/store';
    import { ADICIONA_PROJETO, ALTERA_PROJETO } from '@/store/tipo-multacoes';
    import { TipoNotificacao } from '@/interfaces/INotificacao';
    import useNotificador from '@/hooks/Notificar'

    export default defineComponent({
        name: 'FormularioTasks',
        props: {
            id: {
                type: String
            }
        },
        mounted() {
            if (this.id) {
                const projeto = this.store.state.projetos.find(proj => proj.id == this.id);
                this.nomeDoProjeto = projeto?.nome || ''
            }
        },
        data() {
            return {
                nomeDoProjeto: ''
            }
        },
        methods: {
            salvar() {
                let text = '';
                if (this.id) {
                    this.store.commit(ALTERA_PROJETO, {id: this.id, nome: this.nomeDoProjeto});
                    text = 'O projeto foi editado.';
                } else {
                    this.store.commit(ADICIONA_PROJETO, this.nomeDoProjeto);
                    text = 'Novo projeto foi salvo.';
                }
                this.nomeDoProjeto = '';
                this.notificar(TipoNotificacao.SUCESSO, 'Sucesso', text);
                this.$router.push('/projetos');
            }
        },
        setup() {
            const store = useStore();
            const {notificar} = useNotificador()
            return {
                store,
                notificar
            }
        }
    });
</script>
