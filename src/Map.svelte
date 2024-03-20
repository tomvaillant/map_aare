
<script>
    import { onMount } from 'svelte';
    import * as maptilersdk from '@maptiler/sdk';
    import { camera_positions } from './routes.js';

    let map;
    let activeChapterName = ''; // Tracks the active chapter

  onMount(() => {
    maptilersdk.config.apiKey = 'K3gzOB80giEGLYFtHLTQ';
    const initialPosition = camera_positions[0]; // Initialize with the first camera position

    map = new maptilersdk.Map({
      container: 'map', // container ID
      style: '519c9b05-19d1-47ee-89bb-12e8560ae08c', // your style here
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
          setActiveChapter(positionId);
          break;
        }
      }
    };

    function setActiveChapter(chapterId) {
      if (chapterId === activeChapterName) return;

      const chapterIndex = parseInt(chapterId.split('-')[1]) - 1;
      const position = camera_positions[chapterIndex];
      if (position) {
        map.flyTo({
            ...position,
            freezeElevation: true
        });
        activeChapterName = chapterId;
      }
    }

    function isElementOnScreen(id) {
      const element = document.getElementById(id);
      if (element) {
        const bounds = element.getBoundingClientRect();
        return bounds.top < window.innerHeight && bounds.bottom > 0;
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
@media (max-width: 768px) { /* Adjust 768px based on your definition of mobile devices */
  .scrollytelling {
    grid-template-columns: 80vw;
  }
}
</style>
  
<div id="map"></div>