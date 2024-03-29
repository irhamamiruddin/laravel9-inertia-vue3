<template>
    <AppLayout title="Dashboard">
        <template #header>
            <h2 class="font-semibold text-xl text-gray-800 leading-tight">
                User Edit
            </h2>
        </template>

        <div class="py-12">
            <div class="max-w-7xl mx-auto sm:px-6 lg:px-8">
                <JetFormSection @submitted="updateProfileInformation">
                    <template #title>
                        Profile Information
                    </template>

                    <template #description>
                        Update your account's profile information and email address.
                    </template>

                    <template #form>

                        <!-- Name -->
                        <div class="col-span-6 sm:col-span-4">
                            <JetLabel for="name" value="Name" />
                            <JetInput
                                id="name"
                                v-model="form.name"
                                type="text"
                                class="mt-1 block w-full"
                                autocomplete="name"
                            />
                            <JetInputError :message="form.errors.name" class="mt-2" />
                        </div>

                        <!-- Email -->
                        <div class="col-span-6 sm:col-span-4">
                            <JetLabel for="email" value="Email" />
                            <JetInput
                                id="email"
                                v-model="form.email"
                                type="email"
                                class="mt-1 block w-full"
                            />
                            <JetInputError :message="form.errors.email" class="mt-2" />
                        </div>
                    </template>

                    <template #actions>
                        <JetActionMessage :on="form.recentlySuccessful" class="mr-3">
                            Saved.
                        </JetActionMessage>

                        <JetButton :class="{ 'opacity-25': form.processing }" :disabled="form.processing">
                            Save
                        </JetButton>
                    </template>
                </JetFormSection>
            </div>
        </div>
    </AppLayout>
</template>

<script>
    import { useForm } from '@inertiajs/inertia-vue3';
    import AppLayout from '@/Layouts/AppLayout.vue';
    import JetButton from '@/Jetstream/Button.vue';
    import JetFormSection from '@/Jetstream/FormSection.vue';
    import JetInput from '@/Jetstream/Input.vue';
    import JetInputError from '@/Jetstream/InputError.vue';
    import JetLabel from '@/Jetstream/Label.vue';
    import JetActionMessage from '@/Jetstream/ActionMessage.vue';

    export default{
        components:{
            AppLayout,
            JetActionMessage,
            JetButton,
            JetFormSection,
            JetInput,
            JetInputError,
            JetLabel,
        },

        props:{
            user:Object
        },

        setup(props){
            const form = useForm({
                _method: 'PUT',
                name: props.user.name,
                email: props.user.email,
            });

            const updateProfileInformation = () => {

                form.post(route('users.update',props.user.id), {
                    preserveScroll: true,
                });
            };

            return { form, updateProfileInformation };
        }
    }
</script>

