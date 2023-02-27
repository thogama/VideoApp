<template>
    <div class="col p-0">
        <div id="video-container" class="row">
            <div id="video-tag" class="col">
                <video id="local" class="img-fluid p-3"></video>

            </div>



        </div>
    </div>
    <div class="col p-0">
        <div id="video-container" class="row">
            <div id="video-tag" class="col">
                <video id="remote" class="img-fluid p-3"></video>

            </div>



        </div>
    </div>
</template>

<script lang="ts" setup>

// const remoteVideo: HTMLVideoElement = document.querySelector('video#remote')!;

// Prefer camera resolution nearest to 1280x720.
const constraints = {
    audio: { echoCancellation: true },
    video: { width: 1280, height: 720 }
};

const servers = {
    iceServers: [
        {
            urls: ['stun:stun1.l.google.com:19302', 'stun:stun2.l.google.com:19302'],
        },
    ],
    iceCandidatePoolSize: 10,
};

let remoteStream = new MediaStream();
let localStream = new MediaStream()
const pc = new RTCPeerConnection(servers);
navigator.mediaDevices.getUserMedia(constraints)
    .then((mediaStream) => {
        const video = document.querySelector('video');
        video!.srcObject = mediaStream;
        video!.onloadedmetadata = () => {
            video!.play();
        };
        localStream = mediaStream
    })

localStream.getTracks().forEach((track) => {
    pc.addTrack(track, localStream);
});

pc.ontrack = (event) => {
    event.streams[0].getTracks().forEach((track) => {
        remoteStream.addTrack(track);
    });
};
// remoteVideo.srcObject = remoteStream;
</script>

<style lang="scss" scoped></style>