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


        <Button>Click me</Button>
    </div>
</template>


