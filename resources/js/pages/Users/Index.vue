<script setup lang="ts">
import {
    Table,
    TableBody,
    TableCell,
    TableHead,
    TableHeader,
    TableRow,
} from '@/components/ui/table';
import AppLayout from '@/layouts/AppLayout.vue';
import { type User } from '@/types';
import { Head } from '@inertiajs/vue3';

interface Props {
    users: User[];
}

defineProps<Props>();
</script>

<template>
    <AppLayout>
        <Head title="Index" />
        <Table class="mt-4">
            <TableHeader>
                <TableRow>
                    <TableHead>User</TableHead>
                    <TableHead>Email</TableHead>
                    <TableHead>Role</TableHead>
                    <TableHead class="w-[100px] text-right">Actions</TableHead>
                </TableRow>
            </TableHeader>
            <TableBody>
                <TableRow v-for="user in users" :key="user.id">
                    <TableCell>{{ user.name }}</TableCell>
                    <TableCell>{{ user.email }}</TableCell>
                    <TableCell></TableCell>
                    <TableCell class="text-right"
                        >Button to edit/delete</TableCell
                    >
                </TableRow>
            </TableBody>
        </Table>
    </AppLayout>
</template>
