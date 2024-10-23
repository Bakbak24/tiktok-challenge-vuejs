<script setup>
   const props = defineProps({
    videosrc: String });
</script>

<template>
    <div class="player">
        <video loop autoplay muted :src="videosrc"> </video>
        <div class="player__controls">
            <a href="" class="previous" @click.prevent="$emit('prevVideo')">⬆️</a>
            <a href="" class="next" @click.prevent="$emit('nextVideo')">⬇️</a>
        </div>
    </div>
</template>

<style scoped>
.player__controls {
    display: flex;
    flex-direction: column;
    justify-content: center;
    gap: 20px;    
}
.player__controls a {
    font-size: 2rem;
    text-decoration: none;
}
.player{
    display: flex;
    gap: 20px;
    background-color: black;
    justify-content: center;
}
video {
    max-height: 100vh;
}
</style>
