<template>
    <div>
        <video class="video"></video>
        <button @click="startRecording()">Start Recording</button>
        <button @click="stopRecording()">Stop Recording</button>
        <button @click="download()">Download</button>
    </div>
</template>

<script>
let RecordRTC = require('recordrtc');
    export default{
        data(){
            return {
                stream : '',
                recordRTC : ''
            }
        },
        mounted(){ 
            var recorder = RecordRTC({}, {
                type: 'video',
                recorderType: RecordRTC.WhammyRecorder
            });
        },
        methods:{
            startRecording() {
                let that = this;
                let mediaConstraints = {
                    video: true,
                    audio: true
                };
                navigator.mediaDevices
                    .getUserMedia(mediaConstraints)
                    .then(function(stream) {
                        var options = {
                            mimeType: 'video/webm', // or video/webm\;codecs=h264 or video/webm\;codecs=vp9
                            audioBitsPerSecond: 128000,
                            videoBitsPerSecond: 128000,
                            bitsPerSecond: 128000 // if this line is provided, skip above two
                        };
                        that.stream = stream;
                        that.recordRTC = RecordRTC(stream, options);
                        that.recordRTC.startRecording();
                        let video = document.querySelector('.video');
                        video.src = window.URL.createObjectURL(stream);
                        that.toggleControls();
                    }, function(error) {
                        
                });
            },
            stopRecording(){
                let recordRTC = this.recordRTC;
                recordRTC.stopRecording(this.processVideo(this));
                let stream = this.stream;
                stream.getAudioTracks().forEach(track => track.stop());
                stream.getVideoTracks().forEach(track => track.stop());
            },
            processVideo(audioVideoWebMURL) {
                let video = document.querySelector('.video');
                let recordRTC = this.recordRTC;
                video.src = audioVideoWebMURL;
                this.toggleControls();
                var recordedBlob = recordRTC.getBlob();
                recordRTC.getDataURL(function (dataURL) { });
            },
            download() {
                this.recordRTC.save('video.webm');
            },
            toggleControls() {
                let video = document.querySelector('.video');
                video.muted = !video.muted;
                video.controls = !video.controls;
                video.autoplay = !video.autoplay;
            }
        }
    }
</script>

<style>
  .video {
  box-shadow: 1px 6px 10px 2px rgba(35, 35, 35, 0.62);
  max-height: 800px;
  width: 100%;
}

.row {
  margin-bottom: 20px;
}
</style>
