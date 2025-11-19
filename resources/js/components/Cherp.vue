<script setup lang="ts">
import InputError from '@/components/InputError.vue';
import { Button } from '@/components/ui/button';
import {
    DropdownMenu,
    DropdownMenuContent,
    DropdownMenuItem,
    DropdownMenuTrigger,
} from '@/components/ui/dropdown-menu';
import type { Chirp } from '@/types';
import { router, useForm } from '@inertiajs/vue3';
import dayjs from 'dayjs';
import relativeTime from 'dayjs/plugin/relativeTime';
import { MoreHorizontalIcon } from 'lucide-vue-next';
import { ref } from 'vue';

dayjs.extend(relativeTime);

interface Props {
    chirp: Chirp;
}

const props = defineProps<Props>();

const chirp = props.chirp;

const form = useForm({
    message: props.chirp.message,
});

const editing = ref(false);

const submitForm = () => {
    form.put(route('chirps.update', chirp.id), {
        preserveScroll: true,
        onSuccess: () => (editing.value = false),
    });
};

const deleteChirp = (id: number) => {
    if (confirm('Are you sure you want to delete this task?')) {
        router.delete(route('chirps.destroy', { id }));
    }
};
</script>

<template>
    <div class="flex space-x-2 p-6">
        <svg
            xmlns="http://www.w3.org/2000/svg"
            class="h-6 w-6 -scale-x-100 text-gray-600"
            fill="none"
            viewBox="0 0 24 24"
            stroke="currentColor"
            stroke-width="2"
        >
            <path
                stroke-linecap="round"
                stroke-linejoin="round"
                d="M8 12h.01M12 12h.01M16 12h.01M21 12c0 4.418-4.03 8-9 8a9.863 9.863 0 01-4.255-.949L3 20l1.395-3.72C3.512 15.042 3 13.574 3 12c0-4.418 4.03-8 9-8s9 3.582 9 8z"
            />
        </svg>
        <div class="flex-1">
            <div class="flex items-center justify-between">
                <div>
                    <span class="text-gray-800">{{ chirp.user.name }}</span>
                    <small class="ml-2 text-sm text-gray-600">{{
                        dayjs(chirp.created_at).fromNow()
                    }}</small>
                    <small
                        v-if="chirp.created_at !== chirp.updated_at"
                        class="text-sm text-gray-600"
                    >
                        &middot; edited</small
                    >
                </div>
                <DropdownMenu>
                    <DropdownMenuTrigger
                        ><MoreHorizontalIcon
                    /></DropdownMenuTrigger>
                    <DropdownMenuContent>
                        <DropdownMenuItem @click="editing = true"
                            >Edit</DropdownMenuItem
                        >
                        <DropdownMenuItem @click="deleteChirp(chirp.id)">
                            Delete
                        </DropdownMenuItem>
                    </DropdownMenuContent>
                </DropdownMenu>
            </div>
            <form v-if="editing" @submit.prevent="submitForm()">
                <textarea
                    v-model="form.message"
                    class="focus:ring-opacity-50 mt-4 w-full rounded-md border-gray-300 text-gray-900 shadow-sm focus:border-indigo-300 focus:ring focus:ring-indigo-200"
                ></textarea>
                <InputError :message="form.errors.message" class="mt-2" />
                <div class="space-x-2">
                    <Button :disabled="form.processing">Save</Button>
                    <button
                        class="mt-4"
                        @click="
                            editing = false;
                            form.reset();
                            form.clearErrors();
                        "
                    >
                        Cancel
                    </button>
                </div>
            </form>
            <p v-else class="mt-4 text-lg text-gray-900">{{ chirp.message }}</p>
        </div>
    </div>
</template>
