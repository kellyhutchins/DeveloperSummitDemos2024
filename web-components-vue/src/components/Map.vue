<template>
  <arcgis-map    
    item-id="41ea735861224051818fa810ed2176cf"
    v-on:arcgisViewReadyChange="onReady"
  >
  <arcgis-expand mode="floating" position="top-right">
    <arcgis-search></arcgis-search>
  </arcgis-expand>

  </arcgis-map>
</template>

<script setup>

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