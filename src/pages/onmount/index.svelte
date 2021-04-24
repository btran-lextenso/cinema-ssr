<script>
  import { onMount } from 'svelte';
  import { ready } from '@roxi/routify'
  import { metatags } from '@roxi/routify'

  let photos = [];
  let title

  onMount(async () => {
    const res = await fetch(`https://jsonplaceholder.typicode.com/photos?_limit=20`);
    photos = await res.json();
    console.log(photos[1].title);
    //metatags.title = photos[0].title;
    title = photos[1].title;
  });

//metatags.title = "Title static of my page";
//$metatags.title = photos[0].title;
$: metatags.title = title;

</script>

<style>
  .photos {
    width: 100%;
    display: grid;
    grid-template-columns: repeat(5, 1fr);
    grid-gap: 8px;
  }

  figure, img {
    width: 100%;
    margin: 0;
  }
</style>

<h1>Photo album</h1>

<div class="photos">
  {#each photos as photo}
    <figure>
      <img src={photo.thumbnailUrl} alt={photo.title}>
      <figcaption>{photo.title}</figcaption>
    </figure>
  {:else}
    <!-- this block renders when photos.length === 0 -->
    <p>loading...</p>
  {/each}
</div>
