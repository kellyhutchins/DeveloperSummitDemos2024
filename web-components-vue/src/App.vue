<script setup>
import {ref} from "vue";
import Map from "@/components/Map.vue";
import { defineCustomElements as defineCalciteElements } from "@esri/calcite-components/dist/loader";
import { defineCustomElements } from "@arcgis/map-components/dist/loader";
import "@esri/instant-apps-components/dist/components/instant-apps-splash";
defineCalciteElements(window, {
  resourcesUrl: "https://js.arcgis.com/calcite-components/2.5.1/assets",
});
defineCustomElements(window, { resourcesUrl: "https://js.arcgis.com/map-components/4.29/assets" });

const hasBookmarks = ref(false);
const title=ref("");
const description  = ref("");
const activeTool= ref(hasBookmarks ? 'bookmarks':'legend');
const infoModal = ref(null)
function mapIsReady(map){
  title.value = map.portalItem?.title;
  description.value = map.portalItem.description;
  hasBookmarks.value =map?.bookmarks?.length ? true :false;
  activeTool.value = hasBookmarks.value ? "bookmarks" :"legend";
}
function handleActionBarClick(e){
  if(e.target.tagName !== "CALCITE-ACTION"){
    return;
  }
  activeTool.value = e.target.dataset.actionId;
}
function openInfoModal(){
 infoModal.value.open = true;
}
</script>

<template>
  <calcite-shell >
    <calcite-navigation slot="header">
      <calcite-navigation-logo icon="code" slot="logo" heading="Component Demo" description="Esri Developer Summit 2024">
      </calcite-navigation-logo>
      <calcite-action icon="information" slot="content-end" @click="openInfoModal"></calcite-action>
    </calcite-navigation>
   <calcite-shell-panel slot="panel-start" display-mode="dock">
    <calcite-action-bar ref="actionBarRef" slot="action-bar" @click="handleActionBarClick" >
     <calcite-action data-action-id="bookmarks" text-enabled text="Bookmarks" icon="bookmark" v-if="hasBookmarks"  v-bind:active="(activeTool === 'bookmarks')? true :null"></calcite-action>
      <calcite-action  data-action-id="legend" text-enabled text="Legend" icon="legend" v-bind:active="activeTool === 'legend'? true :null"></calcite-action>
    </calcite-action-bar>
    <!-- Bookmarks Panel -->
    <calcite-panel heading="Bookmarks" v-if="hasBookmarks"  v-bind:closed="activeTool === 'bookmarks' ? null :true" >
      <arcgis-bookmarks  reference-element="arcgis-map" ></arcgis-bookmarks>
    </calcite-panel>
    <!-- Legend Panel -->
    <calcite-panel heading="Legend"  v-bind:closed="activeTool === 'legend'  ? null :true" >
        <arcgis-legend reference-element="arcgis-map"></arcgis-legend>
    </calcite-panel>
   </calcite-shell-panel>
   <!-- Map Panel-->
   <calcite-panel>
    <Map   @mapReady="mapIsReady"/>
   </calcite-panel>
  </calcite-shell>
  <!-- Add splash modal from instant apps components repo -->
  <instant-apps-splash ref="infoModal" :title-text="title" :content="description" primary-button-text="Enter" close-button-disabled="true"  localStorageKey="vueDemo"/>
</template>
<style>
  calcite-panel[closed]{
    display:none;
  }
</style>