<template>
    <GuestLayout title="Dashboard">
        <template #header>
            <h2 class="font-semibold text-xl text-gray-800 leading-tight">
                All Posts
            </h2>
        </template>

        <div class="py-12">
            <div class="max-w-7xl mx-auto sm:px-6 lg:px-8">
                <div class="bg-white overflow-hidden shadow-xl sm:rounded-lg p-3 mb-3"
                v-for="post in posts.data" :key="post">
                    <p class="card-text text-2xl font-bold">{{ post.title }}</p>
                    <p class="card-text text-sm mb-4">@ {{ post.user.name }}</p>
                    <p class="card-text whitespace-pre-line">{{ post.content }}</p>
                </div>
                <JetPagination class="m-5 float-right" :links="posts.links" />
            </div>
        </div>
    </GuestLayout>
</template>

<script>
    import { reactive, watchEffect } from "vue";
    import { pickBy } from "lodash";
    import { Inertia } from "@inertiajs/inertia";
    import GuestLayout from '@/Layouts/GuestLayout.vue';
    import JetPagination from '@/Components/Pagination.vue'

    export default{
        components:{
            GuestLayout,
            JetPagination,
        },

        props: {
            posts: Object,
        },
    }
</script>

