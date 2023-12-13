<script setup>

import AuthenticatedLayout from "@/Layouts/AuthenticatedLayout.vue";
import {Head, useForm} from "@inertiajs/vue3";
import DashboardCard from "@/Components/DashboardCard.vue";
import DashboardCardSection from "@/Components/DashboardCardSection.vue";
import {ref} from "vue";
import { Link } from '@inertiajs/vue3';
import dayjs from "dayjs";
import relativeTime from "dayjs/plugin/relativeTime";
import Dropdown from "@/Components/Dropdown.vue";
import InputError from "@/Components/InputError.vue";
import PrimaryButton from "@/Components/PrimaryButton.vue";
import DropdownLink from "@/Components/DropdownLink.vue";

dayjs.extend(relativeTime)

let form = useForm({
    title: '',
    body: '',
});

function handleBlogEditing(blog) {
    editing.value = blog.id;
    form = useForm({
        id: blog.id,
        title: blog.title,
        body: blog.body,
    });
}

const editing = ref('');
</script>

<template>
    <Head title="Blogs" />

    <AuthenticatedLayout>
        <template #header>
            <h2 class="font-semibold text-xl text-gray-800 dark:text-gray-200 leading-tight inline-block">
                Blogs
            </h2>
            <Link :href="route('blogs.create')" class="float-right text-sm text-gray-800 dark:text-gray-200 my-auto h-full">
                Create new
            </Link>
        </template>

        <dashboard-card>
            <dashboard-card-section
                v-for="blog in $page.props.blogs"
                :key="blog.id"
                class="w-1/3"
            >
                <div class="flex justify-between">
                    <div>
                        <p>
                            {{ blog.title }}
                            <small v-if="blog.created_at != blog.updated_at" class="text-sm text-gray-600">&middot; edited</small>
                        </p>
                        <small class="text-muted">Created {{ dayjs(blog.created_at).fromNow() }}</small>
                    </div>

                    <Dropdown v-if="blog.user_id === $page.props.auth.user.id" :key="blog.id">
                        <template #trigger>
                            <button>
                                <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4 text-gray-400" viewBox="0 0 20 20" fill="currentColor">
                                    <path d="M6 10a2 2 0 11-4 0 2 2 0 014 0zM12 10a2 2 0 11-4 0 2 2 0 014 0zM16 12a2 2 0 100-4 2 2 0 000 4z" />
                                </svg>
                            </button>
                        </template>
                        <template #content>
                            <button
                                class="block w-full px-4 py-2 text-left text-sm leading-5 text-gray-700 hover:bg-gray-100 focus:bg-gray-100 transition duration-150 ease-in-out"
                                @click="handleBlogEditing(blog);"
                                :key="blog.id"
                            >
                                Edit
                            </button>
                            <DropdownLink as="button" :href="route('blogs.destroy', blog.id)" method="delete">
                                Delete
                            </DropdownLink>
                        </template>
                    </Dropdown>
                </div>

                <form
                    v-if="editing === blog.id"
                        @submit.prevent="form.put(route('blogs.update', form.id), { onSuccess: () => editing = '' })"
                >
                    <hr class="my-2">
                    <div class="mb-4">
                        <label for="title" class="block text-gray-700 text-sm font-bold mb-2">Title</label>
                        <input v-model="form.title" type="text" id="title" class="shadow appearance-none border border-gray-300 rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline">
                        <InputError :title="form.title" />
                    </div>

                    <div class="mb-4">
                        <label for="body" class="block text-gray-700 text-sm font-bold mb-2">Content</label>
                        <textarea v-model="form.body" type="text" id="body" class="shadow appearance-none border border-gray-300 rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"></textarea>
                        <InputError :title="form.body" />
                    </div>

                    <div class="space-x-2">
                        <PrimaryButton class="mt-4">Save</PrimaryButton>
                        <button class="mt-4" @click="editing = ''; form.reset(); form.clearErrors()">Cancel</button>
                    </div>
                </form>
            </dashboard-card-section>
        </dashboard-card>
    </AuthenticatedLayout>
</template>
