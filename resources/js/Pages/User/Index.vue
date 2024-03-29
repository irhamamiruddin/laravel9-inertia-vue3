<template>
    <AppLayout title="Dashboard">
        <template #header>
            <h2 class="font-semibold text-xl text-gray-800 leading-tight">
                Index
                <inertia-link :href="route('users.create')"
                v-if='$page.props.permission.users.create'>
                    <JetButton class="float-right">Create</JetButton>
                </inertia-link>
            </h2>
        </template>

        <div class="py-12">
            <div class="max-w-7xl mx-auto sm:px-6 lg:px-8">
                <JetInput
                    type="text"
                    class="block ml-2 mb-4 w-60"
                    v-model="form.search"
                    placeholder="Search user..."
                />
                <div class="bg-white overflow-hidden shadow-xl sm:rounded-lg p-3">
                    <div class="flex flex-col">
                        <div class="overflow-x-auto sm:-mx-6 lg:-mx-8">
                            <div class="py-2 inline-block min-w-full sm:px-6 lg:px-8">
                            <div class="overflow-hidden">
                                <!-- <inertia-link :href="route('users.create')">
                                    <button class="bg-green-500 hover:bg-green-700 text-white font-bold px-4 py-1.5 rounded my-3">
                                        <i class="fa fa-plus" aria-hidden="true"></i> Create
                                    </button>
                                </inertia-link> -->
                                <table class="min-w-full">
                                <thead class="bg-white border-b">
                                    <tr>
                                        <th scope="col" class="text-sm font-medium text-gray-900 px-6 py-4 text-left">
                                            #
                                        </th>
                                        <th scope="col" class="text-sm font-medium text-gray-900 px-6 py-4 text-left">
                                            Name
                                        </th>
                                        <th scope="col" class="text-sm font-medium text-gray-900 px-6 py-4 text-left">
                                            Email
                                        </th>
                                        <th scope="col" class="text-sm font-medium text-gray-900 px-6 py-4 text-left">
                                            Role
                                        </th>
                                        <th scope="col" class="text-sm font-medium text-gray-900 px-6 py-4 text-left">
                                            Actions
                                        </th>
                                    </tr>
                                </thead>
                                <tbody>
                                    <!-- add .data if use paginate -->
                                    <tr v-if="!users.data.length">
                                        <td class="p-4 text-center text-gray-900" colspan="5">
                                            No data
                                        </td>
                                    </tr>
                                    <tr v-for="user in users.data" :key="user">
                                        <td class="text-sm text-gray-900 font-light px-6 py-4 whitespace-nowrap">
                                            {{ user.id }}
                                        </td>
                                        <td class="text-sm text-gray-900 font-light px-6 py-4 whitespace-nowrap">
                                            {{ user.name }}
                                        </td>
                                        <td class="text-sm text-gray-900 font-light px-6 py-4 whitespace-nowrap">
                                            {{ user.email }}
                                        </td>
                                        <td class="text-sm text-gray-900 font-light px-6 py-4 whitespace-nowrap">
                                            {{ user.role }}
                                        </td>
                                        <td>
                                            <inertia-link :href="route('users.show',user.id)" title="View" v-if="user.can.view">
												<button class="inline-block px-2.5 py-2 m-1 bg-blue-400 text-white font-medium text-xs leading-tight uppercase rounded shadow-md hover:bg-blue-500 hover:shadow-lg focus:bg-blue-500 focus:shadow-lg focus:outline-none focus:ring-0 active:bg-blue-600 active:shadow-lg transition duration-150 ease-in-out">
                                                    View
												</button>
											</inertia-link>
											<inertia-link :href="route('users.edit',user.id)" title="Edit" v-if="user.can.update">
												<button class="inline-block px-2.5 py-2 m-1 bg-blue-600 text-white font-medium text-xs leading-tight uppercase rounded shadow-md hover:bg-blue-900 hover:shadow-lg focus:bg-blue-900 focus:shadow-lg focus:outline-none focus:ring-0 active:bg-blue-800 active:shadow-lg transition duration-150 ease-in-out">
                                                    Edit
												</button>
											</inertia-link>
                                            <button
                                                @click="deleteUser(user.id)"
                                                class="inline-block px-2.5 py-2 m-1 bg-red-600 text-white font-medium text-xs leading-tight uppercase rounded shadow-md hover:bg-red-900 hover:shadow-lg focus:bg-red-900 focus:shadow-lg focus:outline-none focus:ring-0 active:bg-red-800 active:shadow-lg transition duration-150 ease-in-out"
                                            >
                                                Delete
                                            </button>
                                        </td>
                                    </tr>
                                </tbody>
                                </table>
                                <JetPagination class="m-5" :links="users.links" />
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
    import {Inertia} from '@inertiajs/inertia';
    import AppLayout from '@/Layouts/AppLayout.vue';
    import JetButton from '@/Jetstream/Button.vue';
    import JetInput from '@/Jetstream/Input.vue';
    import JetPagination from '@/Components/Pagination.vue'

    export default{
        components:
        {
            AppLayout,
            JetButton,
            JetPagination,
            JetInput,
        },

        props:
        {
            users: Object,
            filters: Object,
        },

        methods:
        {
            // To delete Post
            deleteUser(postId)
            {
                const result = confirm("Confirm delete post?");
                if (result) {
                    Inertia.delete(route("posts.destroy", postId));
                }
            },
        },

        setup(props)
        {
            // Make a reactive form. Table change when something is search.
            const form = reactive({
                search: props.filters.search,
                page: props.filters.page,
            });

            watchEffect(() => {
                const query = pickBy(form);
                Inertia.replace(
                    route("users.index", Object.keys(query).length ? query : {})
                );
            });

            // To delete User
            // const deleteUser = (userId) => {
            //     const result = confirm("Confirm delete user?");
            //     if (result) {
            //         Inertia.delete(route("users.destroy", userId), {
            //             preserveScroll: true,
            //         });
            //     }
            // };

            return { form };
        }
    };
</script>
