<template>
    <jet-form-section @submitted="createTeam">
        <template #title>
            Detalhes da Equipe
        </template>

        <template #description>
            Crie um time para colaborar com outros em projetos.
        </template>

        <template #form>
            <div class="col-span-6">
                <jet-label value="Chefe da Equipe" />

                <div class="flex items-center mt-2">
                    <img
                        class="w-12 h-12 rounded-full object-cover"
                        :src="$page.user.profile_photo_url"
                        :alt="$page.user.name"
                    />

                    <div class="ml-4 leading-tight">
                        <div>{{ $page.user.name }}</div>
                        <div class="text-gray-700 text-sm">
                            {{ $page.user.email }}
                        </div>
                    </div>
                </div>
            </div>

            <div class="col-span-6 sm:col-span-4">
                <jet-label for="name" value="Nome da Equipe" />
                <jet-input
                    id="name"
                    type="text"
                    class="mt-1 block w-full"
                    v-model="form.name"
                    autofocus
                />
                <jet-input-error :message="form.error('name')" class="mt-2" />
            </div>
        </template>

        <template #actions>
            <jet-action-message :on="form.recentlySuccessful" class="mr-3">
                Salvo.
            </jet-action-message>

            <jet-button
                :class="{ 'opacity-25': form.processing }"
                :disabled="form.processing"
            >
                Salvar
            </jet-button>
        </template>
    </jet-form-section>
</template>

<script>
import JetActionMessage from "@/Jetstream/ActionMessage";
import JetButton from "@/Jetstream/Button";
import JetFormSection from "@/Jetstream/FormSection";
import JetInput from "@/Jetstream/Input";
import JetInputError from "@/Jetstream/InputError";
import JetLabel from "@/Jetstream/Label";

export default {
    components: {
        JetActionMessage,
        JetButton,
        JetFormSection,
        JetInput,
        JetInputError,
        JetLabel
    },

    data() {
        return {
            form: this.$inertia.form(
                {
                    name: ""
                },
                {
                    bag: "createTeam",
                    resetOnSuccess: false
                }
            )
        };
    },

    methods: {
        createTeam() {
            this.form.post(route("teams.store"), {
                preserveScroll: true
            });
        }
    }
};
</script>
