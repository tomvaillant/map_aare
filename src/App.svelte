<script>
  import Map from './Map.svelte';
  import MapVideo from './Map_video.svelte';
</script>

<main>
  <Map />
  <!-- <MapVideo /> -->
  <div class="scrollytelling">
    <div class="textbox" id="textbox-1">Content 1</div>
    <div class="textbox" id="textbox-2">Content 2</div>
    <div class="textbox" id="textbox-3">Content 3</div>
    <div class="textbox" id="textbox-4">Content 4</div>
    <div class="textbox" id="textbox-5">Content 5</div>
    <div class="textbox" id="textbox-6">Content 6</div>
    <div class="textbox" id="textbox-7">Content 7</div>
    <div class="textbox" id="textbox-8">Content 8</div>
    <div class="textbox" id="textbox-9">Content 9</div>
    <div class="textbox" id="textbox-10">Content 10</div>
    <div class="textbox" id="textbox-11">Content 11</div>
    <div class="textbox" id="textbox-12">Content 12</div>
  </div>
</main>

<style>
  :global(html, body) {
    margin: 0 !important;
    padding: 0 !important;
  }
  main {
    height: 100vh;
    position: relative;
  }

  .scrollytelling {
    display: grid;
    grid-template-rows: repeat(8, 12vh);
    grid-template-columns: 40vw;
    justify-content: center;
    gap: 50vh;
    position: sticky;
    top: 20vh; /* Adjust as needed to control when the scrollytelling starts */
    left: 0;
    right: 0;
    padding: 20px;
    box-sizing: border-box;
  }

  .textbox {
    background: white;
    margin: 10px 0; /* Adjust spacing between textboxes */
    padding: 20px;
    border-radius: 5px;
    box-shadow: 0 4px 6px rgba(0,0,0,0.1);
  }
</style>
