
<script>
    import { onMount } from 'svelte';
    import mapboxgl from 'mapbox-gl';
    import { camera_positions } from './routes.js';

    mapboxgl.accessToken = 'pk.eyJ1IjoiYnVyaWVkc2lnbmFscyIsImEiOiJjbDBhdmlhZTgwM3dtM2RxOTQ5cndsYXl0In0.Gvcq3DBOKDVRhy3QLjImiA';

    onMount(() => {
        // Ensure camera_positions has at least one entry
        if (camera_positions && camera_positions.length > 0) {
            const initialPosition = camera_positions[0];

            const map = new mapboxgl.Map({
                container: 'map', // container ID
                style: 'mapbox://styles/buriedsignals/cltd1sab6001901p7dx5s16ij', // style URL
                center: initialPosition.center, // starting position [lng, lat] from the first camera position
                zoom: initialPosition.zoom, // starting zoom from the first camera position
                bearing: initialPosition.bearing, // starting bearing from the first camera position
                pitch: initialPosition.pitch // starting pitch from the first camera position
            });

            map.on('load', () => {
                let currentSegmentIndex = 0; // Start with the first camera position

                map.addSource('mapbox-dem', {
                    type: 'raster-dem',
                    url: 'mapbox://mapbox.terrain-rgb',
                    tileSize: 512,
                    maxzoom: 14
                });

                map.setTerrain({ source: 'mapbox-dem', exaggeration: 1 });

                window.onscroll = () => {
                    for (let i = 1; i <= camera_positions.length * 2; i++) {
                        const element = document.getElementById(`textbox-${i}`);
                        if (element) {
                            const position = element.getBoundingClientRect();
                            if (position.top < window.innerHeight && position.bottom >= 0) {
                                // Determine new segment index based on the current textbox
                                let newSegmentIndex = Math.floor((i - 1) / 2);
                                if (newSegmentIndex !== currentSegmentIndex && newSegmentIndex < camera_positions.length) {
                                    currentSegmentIndex = newSegmentIndex;
                                    // Trigger the camera movement to the new position
                                    flyToCameraPosition(camera_positions[currentSegmentIndex]);
                                    if (currentSegmentIndex === 1) { // Index 1 corresponds to the second position due to 0-based indexing
                                        // Toggle visibility of the "lakes" layer
                                        map.setLayoutProperty('future_raster', 'visibility', 'visible');
                                    } else {
                                        map.setLayoutProperty('future_raster', 'visibility', 'none');
                                    }
                                }
                                break;
                            }
                        }
                    }
                };
            });

            function flyToCameraPosition(position) {
                map.flyTo(position);
            }
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
  