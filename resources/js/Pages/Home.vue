<template>
    <div>
        <h1>Hello from Inertia + Vue 3</h1>
        <p>{{ message }}</p>
        <div class="bg-blue-500 text-white p-4 rounded">
            Tailwind is working!
        </div>
        <div class="bg-red-600 text-white p-5 rounded">
            Tailwind Test Box
        </div>
    </div>
</template>

<script>
export default {
    props: {
        message: String,
    },
};
</script>
