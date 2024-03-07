<template>
  <arcgis-map    
    item-id="866f3ab5307240d49f62ba95c2f452ca"
    v-on:arcgisViewReadyChange="onReady"
  >
  <arcgis-expand mode="floating" position="top-right">
    <arcgis-search></arcgis-search>
  </arcgis-expand>

  </arcgis-map>
</template>

<script setup >

const emit = defineEmits(["mapReady"]);

function onReady(event) {
  const { map } = event.target;
  emit('mapReady',map);
  
}
</script>
<style>
arcgis-map{
  height:100%;
}
</style>