
<script>
import { onMount } from 'svelte';
import * as maptilersdk from '@maptiler/sdk';
import { chapters } from './routes.js';

let map;

onMount(() => {
    maptilersdk.config.apiKey = 'K3gzOB80giEGLYFtHLTQ';
    const initialPosition = chapters['position-1']; 

    map = new maptilersdk.Map({
    container: 'map',
    style: 'f9d98de4-98b8-41be-b23e-69e77596770a',
    center: [8.24841, 46.56990],
    zoom: initialPosition.zoom,
    bearing: initialPosition.bearing,
    pitch: initialPosition.pitch,
    maxPitch: 85,
    maxZoom: 14,
    terrain: true,
    terrainControl: true
    });

    map.on('load', () => {
        if (map.getLayer('future')) {
            map.setLayoutProperty('future', 'visibility', 'none');
        }
    });

    window.onscroll = function () {
        var chapterNames = Object.keys(chapters);
        for (var i = 0; i < chapterNames.length; i++) {
            var chapterName = chapterNames[i];
            if (isElementOnScreen(chapterName)) {
                setActiveChapter(chapterName);
                break;
            }
        }
    };

    var activeChapterName = 'position-1';
    function setActiveChapter(chapterName) {
        if (chapterName === activeChapterName) return;

        map.flyTo({
            ...chapters[chapterName],
            freezeElevation: true
        });

        document.getElementById(chapterName).classList.add('active');
        document.getElementById(activeChapterName).classList.remove('active');

        activeChapterName = chapterName;

        switch (activeChapterName) {
            case 'position-2':
                setTimeout(() => toggleLayerVisibility('future'), 500);
            break;
            case 'position-4':
                setTimeout(() => toggleLayerVisibility('future'), 500);
            break;
        }
    }

    function toggleLayerVisibility(layerId) {
        const layerVisibility = map.getLayoutProperty(layerId, 'visibility');
        if (layerVisibility === 'visible') {
            map.setLayoutProperty(layerId, 'visibility', 'none');
        } else {
            map.setLayoutProperty(layerId, 'visibility', 'visible');
        }
    }

    function isElementOnScreen(id) {
        var element = document.getElementById(id);
        var bounds = element.getBoundingClientRect();
        return bounds.top < window.innerHeight && bounds.bottom > 0;
    }

});
</script>



<style>
#map {
    position: fixed;
    top: 0;
    bottom: 0;
    width: 100%;
}
</style>
  
<div id="map"></div>