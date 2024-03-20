<script>
  import Map from './Map.svelte';
  import MapVideo from './Map_video.svelte';
</script>

<main>
  <Map />
  <!-- <MapVideo /> -->
  <div class="scrollytelling">
    <div class="textbox" id="position-1">Content</div>
    <div class="textbox">Content 2</div>
    <div class="textbox" id="position-2">Content</div>
    <div class="textbox">Content</div>
    <div class="textbox" id="position-3">Conten</div>
    <div class="textbox">Conten</div>
    <div class="textbox" id="position-4">Conten</div>
    <div class="textbox">Conten</div>
    <div class="textbox" id="position-5">Conten</div>
    <div class="textbox">Content</div>
    <div class="textbox" id="position-6">Content</div>
    <div class="textbox">Content</div>
  </div>
</main>

<style>
  :global(html, body) {
    margin: 0 !important;
    padding: 0 !important;
    background: linear-gradient(
      180deg,
      rgba(222, 244, 250, 1) 0%,
      rgba(255, 255, 255, 1) 49%
    );
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
