<script setup lang="ts">
import Cherp from '@/components/Cherp.vue';
import InputError from '@/components/InputError.vue';
import { Button } from '@/components/ui/button';
import AppLayout from '@/layouts/AppLayout.vue';
import { type Chirp } from '@/types';
import { Head, useForm } from '@inertiajs/vue3';

interface Props {
    chirps: Chirp[];
}

defineProps<Props>();

const form = useForm({
    message: '',
});

const submitForm = () => {
    form.post(route('chirps.store'), {
        preserveScroll: true,
        onSuccess: () => form.reset(),
    });
};
</script>

<template>
    <Head title="Chirps" />

    <AppLayout>
        <div class="mx-auto max-w-2xl p-4 sm:p-6 lg:p-8">
            <form @submit.prevent="submitForm">
                <textarea
                    v-model="form.message"
                    placeholder="What's on your mind?"
                    class="focus:ring-opacity-50 block w-full rounded-md border-gray-300 shadow-sm focus:border-indigo-300 focus:ring focus:ring-indigo-200"
                ></textarea>
                <InputError :message="form.errors.message" class="mt-2" />
                <Button variant="default">Chirp</Button>
            </form>
            <div class="mt-6 divide-y rounded-lg bg-white shadow-sm">
                <Cherp v-for="chirp in chirps" :key="chirp.id" :chirp="chirp" />
            </div>
        </div>
    </AppLayout>
</template>
