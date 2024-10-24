<script setup>
import AuthenticatedLayout from '@/Layouts/AuthenticatedLayout.vue';
import Checkbox from '@/Components/Checkbox.vue';
import InputError from '@/Components/InputError.vue';
import InputLabel from '@/Components/InputLabel.vue';
import PrimaryButton from '@/Components/PrimaryButton.vue';
import TextInput from '@/Components/TextInput.vue';
import { Head, useForm, usePage, Link } from '@inertiajs/vue3';

defineProps({
    task: {
        type: Object,
    },
});

function formatDate(value) {
    return new Date(value).toISOString().split('T')[0];
}

const task = usePage().props.task;

const form = useForm({
    name: task.name,
    description: task.description,
    is_completed: task.is_completed,
    expired_at: task.expired_at ? formatDate(task.expired_at) : null,
});
</script>

<template>

    <Head title="Edit task details" />

    <AuthenticatedLayout>
        <template #header>
            <h2 class="font-semibold text-xl text-gray-800 leading-tight">Edit task details</h2>
        </template>

        <div class="py-12">
            <div class="max-w-7xl mx-auto sm:px-6 lg:px-8">
                <div class="bg-white overflow-hidden shadow-sm sm:rounded-lg">
                    <div class="p-6 text-gray-900">
                        <Link :href="route('tasks.index')" method="get" as="button"
                            class="underline text-sm text-gray-600 hover:text-gray-900 rounded-md focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500">
                        &lt;&nbsp;BACK&nbsp;TO&nbsp;TASKS
                        </Link><br />
                        <form @submit.prevent="form.patch(route('tasks.update', task.id))" class="mt-6 space-y-6">
                            <div>
                                <InputLabel for="name" value="Name" />

                                <TextInput id="name" type="text" class="mt-1 block w-full" v-model="form.name" required
                                    autofocus autocomplete="name" />

                                <InputError class="mt-2" :message="form.errors.name" />
                            </div>

                            <div>
                                <InputLabel for="description" value="Description" />

                                <TextInput id="description" type="text" class="mt-1 block w-full"
                                    v-model="form.description" autocomplete="description" />

                                <InputError class="mt-2" :message="form.errors.description" />
                            </div>

                            <div>
                                <InputLabel for="is_completed" value="Completed" />

                                <Checkbox name="is_completed" v-model:checked="form.is_completed" />

                                <InputError class="mt-2" :message="form.errors.is_completed" />
                            </div>

                            <div>
                                <InputLabel for="expired_at" value="Due Date" />

                                <TextInput id="expired_at" type="date" class="mt-1 block w-full"
                                    v-model="form.expired_at" autofocus autocomplete="expired_at" />

                                <InputError class="mt-2" :message="form.errors.expired_at" />
                            </div>

                            <div class="flex items-center gap-4">
                                <PrimaryButton :disabled="form.processing">Save</PrimaryButton>

                                <Transition enter-active-class="transition ease-in-out" enter-from-class="opacity-0"
                                    leave-active-class="transition ease-in-out" leave-to-class="opacity-0">
                                    <p v-if="form.recentlySuccessful" class="text-sm text-gray-600">Saved.</p>
                                </Transition>
                            </div>
                        </form>
                    </div>
                </div>
            </div>
        </div>
    </AuthenticatedLayout>
</template>
