<script setup>
import {Head, useForm} from "@inertiajs/vue3";
import AuthenticatedLayout from "@/Layouts/AuthenticatedLayout.vue";
import DashboardCard from "@/Components/DashboardCard.vue";
import DashboardCardSection from "@/Components/DashboardCardSection.vue";

const form = useForm({
    title: '',
    body: '',
});
</script>

<template>
    <Head title="Blogs" />

    <AuthenticatedLayout>
        <template #header>
            <h2 class="font-semibold text-xl text-gray-800 dark:text-gray-200 leading-tight">
                Blogs
            </h2>
        </template>

        <dashboard-card>
            <dashboard-card-section>
                <form @submit.prevent="form.post(route('blogs.store'), { onSuccess: () => form.reset() })" class="bg-white shadow-md rounded px-8 pt-6 pb-8 mb-4">
                    <div class="mb-4">
                        <label for="title" class="block text-gray-700 text-sm font-bold mb-2">Title</label>
                        <input v-model="form.title" type="text" id="title" class="shadow appearance-none border border-gray-300 rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline">
                    </div>

                    <div class="mb-4">
                        <label for="body" class="block text-gray-700 text-sm font-bold mb-2">Content</label>
                        <textarea v-model="form.body" type="text" id="body" class="shadow appearance-none border border-gray-300 rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"></textarea>
                    </div>

                    <div class="flex flex-row-reverse">
                        <button class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded">
                            Create
                        </button>
                    </div>
                </form>
            </dashboard-card-section>
        </dashboard-card>
    </AuthenticatedLayout>

</template>
