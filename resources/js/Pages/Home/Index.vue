<script setup>
import { Button } from "@components/ui/button/index.ts";

defineProps({
    message: String
});
</script>
<template>
    <div>
        <h1>Hello from Inertia + Vue 3</h1>
        <p>{{ message }}</p>

        <div class="bg-blue-500 text-white p-4 rounded">
            Tailwind is working!
        </div>

        <Button>Click me</Button>
    </div>
</template>


