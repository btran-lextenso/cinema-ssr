<div class="photo">
  <a href="/stars-du-cinema/{path}">
    {#if indice < 5}
    <img src="https://ba-api.lpnt.fr/images/personne/{img}" alt="" />
    {:else}
    <Lazy height={300}>
      <img src="https://ba-api.lpnt.fr/images/personne/{img}" alt="" />
    </Lazy>
    {/if}
  </a>
</div>
<h2 class="fullname"><a href="/stars-du-cinema/{path}">{fullname}</a></h2>
<small class="date_naissance">{date_naissance.replace(/(\d{4})-(\d{2})-(\d{2})/, '$3/$2/$1')}</small>
<p class="profession">{profession}</p>


<script>
  import Lazy from 'svelte-lazy'
  export let path
  export let img
  export let fullname
  export let date_naissance
  export let profession
  export let indice
</script>
