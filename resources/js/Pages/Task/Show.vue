<script setup>
import AuthenticatedLayout from '@/Layouts/AuthenticatedLayout.vue';
import Checkbox from '@/Components/Checkbox.vue';
import { Link, Head } from '@inertiajs/vue3';

defineProps({
    task: {
        type: Object,
    },
});

const confirmTaskDelete = (id) => {
    if (confirm('Are you sure to delete this Task?')) {
        axios.post(route('tasks.destroy', id), { _method: 'DELETE' })
            .then(window.location.reload())
            .catch(error => { console.log(error); });
    }
};

function formatDate(value) {
    return new Date(value).toISOString().split('T')[0];
}

const toggleCompleted = (id) => {
    axios.post(route('tasks.completed', id))
        .catch(error => { console.log(error); });
};
</script>

<template>

    <Head title="Task details" />

    <AuthenticatedLayout>
        <template #header>
            <h2 class="font-semibold text-xl text-gray-800 leading-tight">Task details</h2>
        </template>

        <div class="py-12">
            <div class="max-w-7xl mx-auto sm:px-6 lg:px-8">
                <div class="bg-white overflow-hidden shadow-sm sm:rounded-lg">
                    <div class="p-6 text-gray-900">
                        <Link :href="route('tasks.index')" method="get" as="button"
                            class="underline text-sm text-gray-600 hover:text-gray-900 rounded-md focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500">
                        &lt;&nbsp;BACK&nbsp;TO&nbsp;TASKS
                        </Link><br /><br />
                        Name: {{ task.name }}<br />
                        Description: {{ task.description }}<br />
                        Completed: <Checkbox name="is_completed" v-model:checked="task.is_completed" @click="toggleCompleted(task.id)" /><br />
                        Expiry date: {{ task.expired_at ? formatDate(task.expired_at) : 'No date set' }}<br />
                        <Link :href="route('tasks.edit', task.id)" method="get" as="button"
                            class="underline text-sm text-gray-600 hover:text-gray-900 rounded-md focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500">
                        EDIT
                        </Link>
                        <Link as="button" @click="confirmTaskDelete(task.id)"
                            class="px-2 underline text-sm text-gray-600 hover:text-gray-900 rounded-md focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500">
                        DELETE
                        </Link>
                    </div>
                </div>
            </div>
        </div>
    </AuthenticatedLayout>
</template>
