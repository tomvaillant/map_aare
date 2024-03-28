<script>
  import Map from './Map.svelte';
  import MapVideo from './Map_video.svelte';
</script>

<main>
  <Map />
  <!-- <MapVideo /> -->
  <div class="scrollytelling">
    <section id="position-1" class="sizer active">
      <div class="textbox">Content</div>
      <div class="textbox">Content</div>
    </section>
    <section id="position-2" class="sizer" >
      <div class="textbox">Content 2</div>
      <div class="textbox">Content</div>
    </section>
    <section id="position-3" class="sizer">
      <div class="textbox">Content</div>
      <div class="textbox">Content</div>
    </section>
    <section id="position-4" class="sizer">
      <div class="textbox">Conten</div>
      <div class="textbox">Conten</div>
    </section>
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
    background-attachment: fixed; 
    background-repeat: no-repeat;
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
    gap: 130vh;
    position: sticky;
    top: 20vh; /* Adjust as needed to control when the scrollytelling starts */
    left: 0;
    right: 0;
    padding: 20px;
    box-sizing: border-box;
  }

  .sizer {
    height: 80vh;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    margin-bottom: 50vh;
  }

  .textbox {
    background: white;
    margin: 10px 0; /* Adjust spacing between textboxes */
    padding: 20px;
    border-radius: 5px;
    box-shadow: 0 4px 6px rgba(0,0,0,0.1);
  }
  @media (max-width: 768px) { /* Adjust 768px based on your definition of mobile devices */
  .scrollytelling {
    grid-template-columns: 80vw;
  }
}
</style>
