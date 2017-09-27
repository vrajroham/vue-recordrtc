<template>
    <div id="app">
        <VRecordrtc>
        </VRecordrtc>
    </div>
</template>

<script>
  import VRecordrtc from '../src/index'
  export default {
    name: 'MainApp',
    components: {
      VRecordrtc
    },
    mounted() {
        console.log("In");
    }
  }
</script>

<style>

</style>
