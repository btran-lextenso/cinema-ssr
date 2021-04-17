{#await getPerson()}
<Loader />

{:then}


<div class="row star ma20">
  <img src="https://ba-api.lpnt.fr/images/personne/{data.content.photo}" alt="" />
  <div class="infos1 ma20">
    <h1>{data.content.nom}</h1>
    <p class="profession"> <b>Profession : </b>{data.content.profession}</p>
    <p class="date_naissance"><b>Date de naissance : </b> {data.content.date_naissance}</p>
    <p class="lieu_naissance"><b>Lieu de naissance : </b> {data.content.lieu_naissance}</p>
    <p class="nationalite"><b>Nationalité : </b> {data.content.nationalite}</p>
    <p class="bio"><b>Mini-bio : </b> {data.content.commentaire}</p>
    <p class="small"><a href={data.content.url_dbpedia}>Sa fiche DBPedia</a></p>
  </div>
</div>

<div class="movies">
  <h2>{Object.keys(movies).length} {Object.keys(movies).length > '1' ? 'films' : 'film' } </h2>

  {#each Object.entries(movies) as [key, value]}
    <dl>
      {#each value.content.pictures as p}
        <dt><img src={p.content.url} width="150" height="150" alt="" /></dt>
      {/each}
      <dd><h3>{value.content.product_title}</h3> <span>- Film n°{key} - ({value.content.movie_duration} min) - {value.content.production_year} </span>
      </dd>
      <p class="description">{value.content.description}</p>

        <dd><b>Fonction ({value.content.fonctions.length}) :</b>
            {#each value.content.fonctions as f}
              <p class="small">
              {f.content.libelle.replace('actor', 'acteur').replace('productor', 'producteur').replace('director', 'directeur')}
              </p>
            {/each}
        </dd>

    </dl>
  {/each}
</div>

{:catch error}
<p class="msg_error_flux">Problème lors du chargement des données. Envoyer ce message à l'administrateur : <br /> {window.location.href} <br /> {error}</p>

{/await}


<script>
import { ready } from '@roxi/routify'
import { metatags } from '@roxi/routify'
import Loader from '../_components/Loader.svelte'

export let personne

let data = {},
    movies = {};

async function getPerson() {
  const response = await fetch(`https://www.devinez-le-film.fr/personnes/${personne}.json`);
  data = await response.json()
  movies = data.content.movies
  $ready()
  return data.message
}

metatags.title = "Cinéma - page star de cinéma "
metatags.description = 'Description star de cinéma'

</script>
