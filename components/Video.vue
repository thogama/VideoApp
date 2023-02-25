<template>
    <div class="col-sm-8 p-0">
        <div id="video-container" class="row">
            <div id="video-tag" class="col">
                <video class="img-fluid p-3"></video>
                
            </div>


        </div>
    </div>
</template>

<script lang="ts" setup>
// Prefer camera resolution nearest to 1280x720.
const constraints = {
    audio: true,
    video: { width: 1280, height: 720 }
};

navigator.mediaDevices.getUserMedia(constraints)
    .then((mediaStream) => {
        const video = document.querySelector('video');
        video!.srcObject = mediaStream;
        video!.onloadedmetadata = () => {
            video!.play();
        };
    })
    .catch((err) => {
        // always check for errors at the end.
        console.error(`${err.name}: ${err.message}`);
    });
</script>

<style lang="scss" scoped></style>