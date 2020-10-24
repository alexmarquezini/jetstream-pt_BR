<template>
    <jet-action-section>
        <template #title>
            Excluir Equipe
        </template>

        <template #description>
            Excluir este time permanentemente.
        </template>

        <template #content>
            <div class="max-w-xl text-sm text-gray-600">
                Uma vez excluído, o time e todos os seus dados e recursos serão
                excluídos permanentemente. Antes de excluir este time, faça
                download de todos os dados e informações sobre este time que
                você deseje manter.
            </div>

            <div class="mt-5">
                <jet-danger-button @click.native="confirmTeamDeletion">
                    Excluir Equipe
                </jet-danger-button>
            </div>

            <!-- Delete Team Confirmation Modal -->
            <jet-confirmation-modal
                :show="confirmingTeamDeletion"
                @close="confirmingTeamDeletion = false"
            >
                <template #title>
                    Excluir Equipe
                </template>

                <template #content>
                    Tem certeza que quer excluir esta equipe? Uma vez excluído,
                    todos os dados e recursos relativos a ela serão excluídos
                    permanentemente.
                </template>

                <template #footer>
                    <jet-secondary-button
                        @click.native="confirmingTeamDeletion = false"
                    >
                        Deixa pra lá!
                    </jet-secondary-button>

                    <jet-danger-button
                        class="ml-2"
                        @click.native="deleteTeam"
                        :class="{ 'opacity-25': form.processing }"
                        :disabled="form.processing"
                    >
                        Excluir Equipe
                    </jet-danger-button>
                </template>
            </jet-confirmation-modal>
        </template>
    </jet-action-section>
</template>

<script>
import JetActionSection from "@/Jetstream/ActionSection";
import JetButton from "@/Jetstream/Button";
import JetConfirmationModal from "@/Jetstream/ConfirmationModal";
import JetDangerButton from "@/Jetstream/DangerButton";
import JetSecondaryButton from "@/Jetstream/SecondaryButton";

export default {
    props: ["team"],

    components: {
        JetActionSection,
        JetButton,
        JetConfirmationModal,
        JetDangerButton,
        JetSecondaryButton
    },

    data() {
        return {
            confirmingTeamDeletion: false,
            deleting: false,

            form: this.$inertia.form(
                {
                    //
                },
                {
                    bag: "deleteTeam"
                }
            )
        };
    },

    methods: {
        confirmTeamDeletion() {
            this.confirmingTeamDeletion = true;
        },

        deleteTeam() {
            this.form.delete(route("teams.destroy", this.team), {
                preserveScroll: true
            });
        }
    }
};
</script>
