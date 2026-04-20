<template>
  <!-- Grid managing tab content -->
  <div uk-grid class="uk-height-1-1 uk-grid-divider uk-margin-remove uk-padding-remove">
    <div class="control-panel">
      <offsetControl />
    </div>
    <div class="view-component uk-width-expand">
      <offsetChart />
      <simpleSurface />
    </div>
  </div>
</template>

<script>
import offsetControl from './offsetComponents/offsetControl.vue'
import offsetChart from './offsetComponents/offsetChart.vue'
import simpleSurface from './offsetComponents/simpleSurface.vue'

export default {
  name: 'ControlContent',

  components: {
    offsetControl,
    offsetChart,
    simpleSurface,
  },
}
</script>

<style scoped>
.control-panel {
  width: 450px;
}
</style>
