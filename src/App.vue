<script setup>
import { onMounted, ref, reactive } from 'vue';

import VideoPlayer from './components/Video/VideoPlayer.vue'
import VideoDetails from './components/Video/VideoDetails.vue'
import Chat from './components/Messages/Chat.vue'

const currentTitle = ref('Video Title');
const currentVideo = ref('');
const videos = reactive([]);
const currentVideoIndex = ref(0);

onMounted(() => {
    fetch('https://api.jsonbin.io/v3/b/670ccbfaad19ca34f8b81951')
    .then(res => res.json())
    .then(data => {
        videos.value = data.record.data.videos;
        currentVideo.value = data.record.data.videos[0].video;
        currentTitle.value = data.record.data.videos[0].description;
        //currentUsername.value = data.record.data.videos[0].username;

    })
})


function nextVideo(){
    currentVideoIndex.value++;
    if(currentVideoIndex.value >= videos.value.length){
        currentVideoIndex.value = 0;
    }
    currentVideo.value = videos.value[currentVideoIndex.value].video;
    currentTitle.value = videos.value[currentVideoIndex.value].description;

}

function prevVideo(){
    currentVideoIndex.value--;
    if(currentVideoIndex.value < 0){
        currentVideoIndex.value = videos.value.length - 1;
    }
    currentVideo.value = videos.value[currentVideoIndex.value].video;
    currentTitle.value = videos.value[currentVideoIndex.value].description;

}

</script>

<template>
    <div class="grid">
        <div class="player">
            <VideoPlayer @prevVideo="prevVideo" @nextVideo="nextVideo" :videosrc="currentVideo"/>
        </div>
        <div class="messages">
            <VideoDetails :title="currentTitle" />
            <Chat />
        </div>
    </div>

</template>

<style scoped>
.grid {
    font-family: 'Courier New', Courier, monospace;
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 20px;
    height: 100vh;

}
.messages{
    margin: 20px;
}

</style>
