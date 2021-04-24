{#await getPerson()}
<Loader />
{:then}
<Bio photo={data.content.photo} nom={data.content.nom} profession={data.content.profession} date_naissance={data.content.date_naissance}
lieu_naissance={data.content.lieu_naissance} nationalite={data.content.nationalite} commentaire={data.content.commentaire} url_dbpedia={data.content.url_dbpedia} />

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
<p class="msg_error_flux">Problème lors du chargement des données. Envoyez ce message à l'administrateur : <br /> {window.location.href} <br /> {error}</p>
{/await}


<script>
import { ready } from '@roxi/routify'
import { metatags } from '@roxi/routify'
import Loader from '../_components/Loader.svelte'
import Bio from '../_components/Bio.svelte'
export let personne

let data = {},
    movies = {},
    meta_title,
    meta_description;

async function getPerson() {
  const response = await fetch(`https://www.devinez-le-film.fr/personnes/${personne}.json`);
  data = await response.json()
  movies = data.content.movies
  meta_title = data.content.nom
  meta_description = data.content.commentaire.substring(0,150)
  $ready()
  return data
}

$: metatags.title = meta_title + " - Cinéma SSR"
$: metatags.description = meta_description

</script>
