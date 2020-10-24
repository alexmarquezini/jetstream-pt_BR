<template>
    <jet-action-section>
        <template #title>
            Excluir Conta
        </template>

        <template #description>
            Excluir esta conta permanentemente.
        </template>

        <template #content>
            <div class="max-w-xl text-sm text-gray-600">
                Uma vez que sua conta é excluída, todos os dados e recursos
                desta conta serão excluídos permanentemente. Antes de excluir a
                sua conta, faça download de todos os dados e informações que
                você deseje manter.
            </div>

            <div class="mt-5">
                <jet-danger-button @click.native="confirmUserDeletion">
                    Excluir Conta
                </jet-danger-button>
            </div>

            <!-- Delete Account Confirmation Modal -->
            <jet-dialog-modal
                :show="confirmingUserDeletion"
                @close="confirmingUserDeletion = false"
            >
                <template #title>
                    Excluir Conta
                </template>

                <template #content>
                    Tem certeza que quer excluir a sua conta? Uma vez excluída,
                    todos os dados e recursos relativos a ela serão
                    permanentemente excluídos. Por favor informe a sua senha
                    para confirmar que você deseja excluir permanentemente a sua
                    conta.

                    <div class="mt-4">
                        <jet-input
                            type="password"
                            class="mt-1 block w-3/4"
                            placeholder="Senha"
                            ref="password"
                            v-model="form.password"
                            @keyup.enter.native="deleteUser"
                        />

                        <jet-input-error
                            :message="form.error('password')"
                            class="mt-2"
                        />
                    </div>
                </template>

                <template #footer>
                    <jet-secondary-button
                        @click.native="confirmingUserDeletion = false"
                    >
                        Deixa pra lá!
                    </jet-secondary-button>

                    <jet-danger-button
                        class="ml-2"
                        @click.native="deleteUser"
                        :class="{ 'opacity-25': form.processing }"
                        :disabled="form.processing"
                    >
                        Excluir Conta
                    </jet-danger-button>
                </template>
            </jet-dialog-modal>
        </template>
    </jet-action-section>
</template>

<script>
import JetActionSection from "@/Jetstream/ActionSection";
import JetButton from "@/Jetstream/Button";
import JetDialogModal from "@/Jetstream/DialogModal";
import JetDangerButton from "@/Jetstream/DangerButton";
import JetInput from "@/Jetstream/Input";
import JetInputError from "@/Jetstream/InputError";
import JetSecondaryButton from "@/Jetstream/SecondaryButton";

export default {
    components: {
        JetActionSection,
        JetButton,
        JetDangerButton,
        JetDialogModal,
        JetInput,
        JetInputError,
        JetSecondaryButton
    },

    data() {
        return {
            confirmingUserDeletion: false,
            deleting: false,

            form: this.$inertia.form(
                {
                    _method: "DELETE",
                    password: ""
                },
                {
                    bag: "deleteUser"
                }
            )
        };
    },

    methods: {
        confirmUserDeletion() {
            this.form.password = "";

            this.confirmingUserDeletion = true;

            setTimeout(() => {
                this.$refs.password.focus();
            }, 250);
        },

        deleteUser() {
            this.form
                .post(route("current-user.destroy"), {
                    preserveScroll: true
                })
                .then(response => {
                    if (!this.form.hasErrors()) {
                        this.confirmingUserDeletion = false;
                    }
                });
        }
    }
};
</script>
