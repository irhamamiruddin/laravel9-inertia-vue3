<template>
    <AppLayout title="Dashboard">
        <template #header>
            <h2 class="font-semibold text-xl text-gray-800 leading-tight">
                Edit Post
            </h2>
        </template>

        <div class="py-12">
            <div class="max-w-7xl mx-auto sm:px-6 lg:px-8">
                <JetFormSection @submitted="updatePostInformation(post)">
                    <template #title>
                        Post Update
                    </template>

                    <template #description>
                        Update your post's contents.
                    </template>

                    <template #form>

                        <!-- Title -->
                        <div class="col-span-6 sm:col-span-4">
                            <JetLabel for="title" value="Title" />
                            <JetInput
                                id="title"
                                v-model="post.title"
                                type="text"
                                class="mt-1 block w-full"
                                autocomplete="title"
                            />
                            <JetInputError :message="$page.props.errors.title" class="mt-2" />
                        </div>

                        <!-- Content -->
                        <div class="col-span-6 sm:col-span-4">
                            <JetLabel for="content" value="Content" />
                            <textarea
                                class="mt-1 block w-full border-gray-300 focus:border-indigo-300 focus:ring focus:ring-indigo-200 focus:ring-opacity-50 rounded-md shadow-sm"
                                id="content"
                                v-model="post.content"
                                row="10">
                            </textarea>
                            <JetInputError :message="$page.props.errors.content" class="mt-2" />
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
    // import { useForm } from '@inertiajs/inertia-vue3';
    import { Inertia } from '@inertiajs/inertia';
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

        props:
        {
            post: Object,
        },

        data(props){
            return{
                form:{
                    title: props.post.title,
                    content: props.post.content,
                }
            }
        },

        methods:
        {
            updatePostInformation: function (data) {
                data._method = 'PUT';
                Inertia.post('/posts/' + data.id, data);
                // Inertia.post(route('posts.update',data));
            }
        },

        // setup(props){
        //     const form = useForm({
        //         _method: 'PUT',
        //         title: props.post.title,
        //         content: props.post.content,
        //     });

        //     const updatePostInformation = () => {

        //         form.post(route('posts.update',props.post.id), {
        //             preserveScroll: true,
        //         });
        //     };

        //     return { form, updatePostInformation };
        // }
    }
</script>
