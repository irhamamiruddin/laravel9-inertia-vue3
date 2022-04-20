<template>
    <AppLayout title="Dashboard">
        <template #header>
            <h2 class="font-semibold text-xl text-gray-800 leading-tight">
                Post Index
                <inertia-link :href="route('posts.create')"
                v-if='$page.props.permission.posts.create'>
                    <JetButton class="float-right">Create</JetButton>
                </inertia-link>
            </h2>
        </template>

        <div class="py-12">
            <div class="max-w-7xl mx-auto sm:px-6 lg:px-8">

                <div class="bg-white overflow-hidden shadow-xl sm:rounded-lg p-3">
                    <div class="flex flex-col">
                        <div class="overflow-x-auto sm:-mx-6 lg:-mx-8">
                            <div class="py-2 inline-block min-w-full sm:px-6 lg:px-8">
                                <div class="overflow-hidden">
                                    <table class="min-w-full">
                                        <thead class="bg-white border-b">
                                            <tr>
                                                <th scope="col" class="text-sm font-medium text-gray-900 px-6 py-4 text-left">
                                                    Id
                                                </th>
                                                <th scope="col" class="text-sm font-medium text-gray-900 px-6 py-4 text-left">
                                                    Title
                                                </th>
                                                <th scope="col" class="text-sm font-medium text-gray-900 px-6 py-4 text-left">
                                                    Created At
                                                </th>
                                                <th scope="col" class="text-sm font-medium text-gray-900 px-6 py-4 text-left">
                                                    Updated At
                                                </th>
                                                <th scope="col" class="text-sm font-medium text-gray-900 px-6 py-4 text-left">
                                                    Actions
                                                </th>
                                            </tr>
                                        </thead>
                                        <tbody>
                                            <!-- <tr v-if="!posts.data.length">
                                                <td class="p-4 text-center text-gray-900" colspan="5">
                                                    No data
                                                </td>
                                            </tr> -->
                                            <tr v-for="post in posts" :key="post">
                                                <td class="text-sm text-gray-900 font-light px-6 py-4 whitespace-nowrap">
                                                    {{ post.id }}
                                                </td>
                                                <td class="text-sm text-gray-900 font-light px-6 py-4 whitespace-nowrap">
                                                    {{ post.title }}
                                                </td>
                                                <td class="text-sm text-gray-900 font-light px-6 py-4 whitespace-nowrap">
                                                    {{ post.created_at }}
                                                </td>
                                                <td class="text-sm text-gray-900 font-light px-6 py-4 whitespace-nowrap">
                                                    {{ post.updated_at }}
                                                </td>
                                                <td>
                                                    <inertia-link :href="route('posts.show',post.id)" title="View" v-if="post.can.view">
                                                        <button class="inline-block px-2.5 py-2 m-1 bg-blue-400 text-white font-medium text-xs leading-tight uppercase rounded shadow-md hover:bg-blue-500 hover:shadow-lg focus:bg-blue-500 focus:shadow-lg focus:outline-none focus:ring-0 active:bg-blue-600 active:shadow-lg transition duration-150 ease-in-out">
                                                            View
                                                        </button>
                                                    </inertia-link>
                                                    <inertia-link :href="route('posts.edit',post.id)" title="Edit" v-if="post.can.update">
                                                        <button class="inline-block px-2.5 py-2 m-1 bg-blue-600 text-white font-medium text-xs leading-tight uppercase rounded shadow-md hover:bg-blue-900 hover:shadow-lg focus:bg-blue-900 focus:shadow-lg focus:outline-none focus:ring-0 active:bg-blue-800 active:shadow-lg transition duration-150 ease-in-out">
                                                            Edit
                                                        </button>
                                                    </inertia-link>
                                                    <button
                                                        @click="deletePost(post.id)"
                                                        class="inline-block px-2.5 py-2 m-1 bg-red-600 text-white font-medium text-xs leading-tight uppercase rounded shadow-md hover:bg-red-900 hover:shadow-lg focus:bg-red-900 focus:shadow-lg focus:outline-none focus:ring-0 active:bg-red-800 active:shadow-lg transition duration-150 ease-in-out"
                                                        v-if="post.can.delete"
                                                    >
                                                        Delete
                                                    </button>
                                                </td>
                                            </tr>
                                        </tbody>
                                    </table>
                                    <!-- <jet-pagination class="m-5" :links="posts.links" /> -->
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </AppLayout>
</template>
<script>
    import { reactive, watchEffect } from "vue";
    import { pickBy } from "lodash";
    import { Inertia } from "@inertiajs/inertia";
    import AppLayout from '@/Layouts/AppLayout.vue';
    import JetButton from '@/Jetstream/Button.vue';
    import JetInput from '@/Jetstream/Input.vue';
    // import JetPagination from '@/Components/JetPagination.vue'

    export default{
        components: {
            AppLayout,
            JetButton,
            JetInput,
            // JetPagination,
        },

        props: {
            posts: Object,
            filters: Object,
        },

        setup(props) {

            const form = reactive({
                // search: props.filters.search,
                // page: props.filters.page,
            });

            watchEffect(() => {
                const query = pickBy(form);

                Inertia.replace(
                    route("posts.index", Object.keys(query).length ? query : {})
                );
            });

            const deletePost = (postId) => {

                const result = confirm("Sure to delete?");

                if (result) {
                    Inertia.delete(route("posts.destroy", postId), {
                    preserveScroll: true,
                    });
                }
            };

            return { form, deletePost };
        }
    }

</script>
