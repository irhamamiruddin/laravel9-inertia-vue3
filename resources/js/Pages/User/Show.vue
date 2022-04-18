<template>
    <AppLayout title="Dashboard">
        <template #header>
            <h2 class="font-semibold text-xl text-gray-800 leading-tight">
                Show
            </h2>
        </template>

        <div class="py-12">
            <div class="max-w-7xl mx-auto sm:px-6 lg:px-8">
                <div class="bg-white overflow-hidden shadow-xl sm:rounded-lg p-3">
                    <p class="card-text">Name : {{ users.name}}</p>
                    <p class="card-text">Email : {{ users.email}}</p>
                    <p class="card-text">Role : {{ users.role}}</p>
                    <p class="card-text">Created At : {{ users.created_at}}</p>
                    <p class="card-text">Updated At : {{ users.updated_at}}</p>
                </div>
            </div>
        </div>
    </AppLayout>
</template>

<script>
    import AppLayout from '@/Layouts/AppLayout.vue';

    export default{
        components:{
            AppLayout,
        },

        props:['users']
    }
</script>

