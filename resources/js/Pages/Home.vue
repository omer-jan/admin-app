<template>
    <div>
        <h1>Hello from Inertia + Vue 3</h1>
        <p>{{ message }}</p>
    </div>
</template>

<script>
export default {
    props: {
        message: String,
    },
};
</script>
