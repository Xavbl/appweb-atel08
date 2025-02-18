<script setup lang="ts">
import type { TimelinePost } from '../posts';
    
    //const props = 
    defineProps<{post: TimelinePost}>()
</script>
<template>
    <li>
    <span>{{ post.title }}</span>&nbsp;|&nbsp;
    <span>{{ post.created.toFormat("d MMM") }}</span>
    </li>
</template>
<style>
</style>