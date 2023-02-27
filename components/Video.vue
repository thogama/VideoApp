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
import firebase from 'firebase/compat/app'; //v9
import 'firebase/compat/firestore'; //v9
const remoteVideo: HTMLVideoElement = document.querySelector('video#remote')!;

// Prefer camera resolution nearest to 1280x720.
const constraints = {
    audio: { echoCancellation: true },
    video: { width: 1280, height: 720 }
};
const firestore = firebase.firestore();

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
remoteVideo.srcObject = remoteStream;

const callDoc = firestore.collection('calls').doc();
const offerCandidates = callDoc.collection('offerCandidates');
const answerCandidates = callDoc.collection('answerCandidates');
const callInput = 220000
callInput.value = callDoc.id;

// Get candidates for caller, save to db
pc.onicecandidate = (event) => {
    event.candidate && offerCandidates.add(event.candidate.toJSON());
};

// Create offer
const offerDescription = await pc.createOffer();
await pc.setLocalDescription(offerDescription);

const offer = {
    sdp: offerDescription.sdp,
    type: offerDescription.type,
}
</script>

<style lang="scss" scoped></style>