<template>
    <div>
        Hi
    </div>
</template>

<script>
let RecordRTC = require('recordrtc');
    export default{
        mounted(){ 
            var recorder = RecordRTC({}, {
                type: 'video',
                recorderType: RecordRTC.WhammyRecorder
            });

            console.log('\n--------\nRecordRTC\n--------\n');
            console.log(recorder);
        }
    }
</script>

<style>
  
</style>
