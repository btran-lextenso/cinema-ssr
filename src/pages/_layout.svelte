<header>
  <h1 class="txtcenter"><a href="/"><img src="/images/palme.svg" width="145" height="101" alt="Logo" />Cinéma</a></h1>
</header>

<main id="main">
  <slot />
</main>

<BackToTop />
<PageProgress color="linear-gradient(90deg, yellow 0%, #c8a461 100%)" height="2vh" />

<footer>
  <p class="txtcenter">FOOTER</p>
</footer>

<script>
  import BackToTop from './_components/BackToTop.svelte'
  import PageProgress from "svelte-page-progress"
</script>
