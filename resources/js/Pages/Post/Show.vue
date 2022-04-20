<template>
    <GuestLayout title="Dashboard">
        <template #header>
            <h2 class="font-semibold text-xl text-gray-800 leading-tight">
                Show Post
            </h2>
        </template>

        <div class="py-12">
            <div class="max-w-7xl mx-auto sm:px-6 lg:px-8">
                <div class="bg-white overflow-hidden shadow-xl sm:rounded-lg p-3">
                    <p class="card-text text-2xl font-bold">{{ post.title }}</p>
                    <p class="card-text text-sm mb-4">@ {{ post.user.name }}</p>
                    <p class="card-text whitespace-pre-line">{{ post.content }}</p>
                </div>
            </div>
        </div>
    </GuestLayout>
</template>

<script>
    import GuestLayout from '@/Layouts/GuestLayout.vue';

    export default{
        components:{
            GuestLayout,
        },

        props:['post']
    }
</script>

