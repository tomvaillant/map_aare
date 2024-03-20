
<script>
import { onMount } from 'svelte';
import * as maptilersdk from '@maptiler/sdk';
import { camera_positions } from './routes.js';

let map;
let activeChapterName = '';

onMount(() => {
    maptilersdk.config.apiKey = 'K3gzOB80giEGLYFtHLTQ';
    const initialPosition = camera_positions[0]; 

    map = new maptilersdk.Map({
    container: 'map',
    style: '484efd85-403c-4c11-9165-6f827099fcbf',
    center: [8.19510, 46.56919],
    zoom: initialPosition.zoom,
    bearing: initialPosition.bearing,
    pitch: initialPosition.pitch,
    maxPitch: 85,
    maxZoom: 14,
    terrain: true,
    terrainControl: true
    });

    window.onscroll = () => {
        for (let i = 1; i <= camera_positions.length; i++) {
            let positionId = `position-${i}`;
            if (isElementOnScreen(positionId)) {
            updateChapter(positionId);
            break;
            }
        }
    };

    function updateChapter(positionId) {
        if (activeChapterName === positionId) return;

        const chapterIndex = parseInt(positionId.split('-')[1]) - 1;
        const position = camera_positions[chapterIndex];
        if (position) {
            map.flyTo({ 
                ...position, 
                freezeElevation: true 
            });
        activeChapterName = positionId;
        }
        switch (activeChapterName) {
        case 'position-2':
            toggleLayerVisibility('future');
            console.log("logging position 2");
        break;
        case 'position-4':
            toggleLayerVisibility('future');
        break;
        }
    }

    function toggleLayerVisibility(layerId) {
        const layerVisibility = map.getLayoutProperty(layerId, 'visibility');
        console.log(layerVisibility);
        if (layerVisibility === 'visible') {
            map.setLayoutProperty(layerId, 'visibility', 'none');
        } else {
            map.setLayoutProperty(layerId, 'visibility', 'visible');
        }
    }

    function isElementOnScreen(id) {
        const element = document.getElementById(id);
        if (element) {
            const bounds = element.getBoundingClientRect();
            return bounds.top < window.innerHeight && bounds.bottom >= 0;
        }
        return false;
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