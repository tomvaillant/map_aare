
<script>
    import { onMount } from 'svelte';
    import mapboxgl from 'mapbox-gl';
    import { camera_positions } from './routes.js';

    mapboxgl.accessToken = 'pk.eyJ1IjoiYnVyaWVkc2lnbmFscyIsImEiOiJjbDBhdmlhZTgwM3dtM2RxOTQ5cndsYXl0In0.Gvcq3DBOKDVRhy3QLjImiA';

    function lerp(r,n,t){if(Array.isArray(r)&&Array.isArray(n)){const e=[];for(let a=0;a<Math.min(r.length,n.length);a++)e[a]=r[a]*(1-t)+n[a]*t;return e}return r*(1-t)+n*t}
    function animate(n){let t=null,o=null,e=0,u=0;const a=function(c){o=requestAnimationFrame(a);const i=c-t;t=c,e+=i;const{duration:l,update:r}=n[u];e>l?(u++,u%=n.length,e-=l):r(e/l)};function c(){o&&cancelAnimationFrame(o),o=null}return{play:function(){o||(t=performance.now(),a(t))},pause:c,stop:function(){c(),e=0,u=0}}}

    onMount(() => {
        var animation = null;
        
        var map = new mapboxgl.Map({
            container: 'map',
            zoom: 1,
            center: [0, 0],
            style: 'mapbox://styles/buriedsignals/cltd1sab6001901p7dx5s16ij'
        });

        map.on('load', function() {
            map.addSource('mapbox-dem', {
                type: 'raster-dem',
                url: 'mapbox://mapbox.terrain-rgb',
                tileSize: 512,
                maxzoom: 14
            });
            map.setTerrain({ source: 'mapbox-dem', exaggeration: 1 });
            map.addLayer({
                id: 'sky',
                type: 'sky',
                paint: {
                'sky-type': 'atmosphere',
                'sky-opacity': [
                    'interpolate',
                    ['exponential', 0.1],
                    ['zoom'],
                    5,
                    0,
                    22,
                    1
                ]
                }
            });
            map.setPaintProperty('satellite', 'raster-fade-duration', 0);

            const start = {
                position: [0.5230603065128312, 0.35297719487069595, 0.00017994359220157915],
                target: [8.077540368414418, 46.75976991847591],
                targetElevation: 801.1455533319212,
                exaggeration: 1,
            };

            function setMapValues(values) {
                const {
                position,
                target,
                targetElevation,
                exaggeration,
                } = values;

                const camera = map.getFreeCameraOptions();
                camera.position = new mapboxgl.MercatorCoordinate(position[0], position[1], position[2]);

                let originalGetAtPoint = null;
                if (camera._elevation) {
                originalGetAtPoint = camera._elevation.getAtPoint;
                camera._elevation.getAtPoint = () => targetElevation;
                }
                camera.lookAtPoint(target);
                // Restore to former glory.
                if (camera._elevation) {
                camera._elevation.getAtPoint = originalGetAtPoint;
                }

                map.setFreeCameraOptions(camera);
                map.setTerrain({
                source: 'mapbox-dem',
                exaggeration: exaggeration
                });
            }
            // Prepare the map for the initial position.
            setMapValues(start);

            function getNextIndex(index) {
                // Get the next index, wrapping around to the start if necessary
                return (index + 1) % camera_positions.length;
            }

            animation = animate([{
                duration: 5000 * (camera_positions.length - 1), // Adjust duration based on number of positions
                update: function(phase) {
                    // Calculate the current segment based on the phase and total duration
                    const segmentLength = 1 / (camera_positions.length - 1);
                    let currentSegment = Math.floor(phase / segmentLength);
                    let localPhase = (phase % segmentLength) / segmentLength;

                    // Ensure we don't go out of bounds on the last segment
                    if (currentSegment >= camera_positions.length - 1) {
                        currentSegment = camera_positions.length - 2;
                        localPhase = 1; // End of the last segment
                    }

                    // Determine the start and end positions for the current phase
                    const start = camera_positions[currentSegment];
                    const end = camera_positions[getNextIndex(currentSegment)];

                    // Interpolate all the values between start and end based on localPhase
                    const values = {};
                    Object.keys(start).forEach(key => {
                        // Assuming start and end have the same structure and keys
                        if (typeof start[key] === 'number') {
                            // For numbers directly
                            values[key] = lerp(start[key], end[key], localPhase);
                        } else if (Array.isArray(start[key])) {
                            // For arrays (like position and target), interpolate each element
                            values[key] = start[key].map((startVal, index) => lerp(startVal, end[key][index], localPhase));
                        }
                    });

                    setMapValues(values);
                }
            }]);

            animation.play();
        })
    })
</script>

<style>
#map {
    position: absolute;
    top: 0;
    bottom: 0;
    width: 100%;
}
</style>
  
<div id="map"></div>
  