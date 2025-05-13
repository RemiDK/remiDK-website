<script setup lang="ts">
defineProps<{
    name: string
}>()
</script>

<template>
    <div class="skill">
        {{ name }}
    </div>
</template>