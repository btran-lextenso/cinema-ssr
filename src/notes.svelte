var foo = {
  'nom' : 'toto',
  'age' : 11
}
console.log('>>>', Object.values(foo))

récupérer les valeurs d'un objet et met dans un tableau
{#each Object.values(data.content.movies) as movie}
<p>{movie.content.product_title}</p>
<p>{movie.content.movie_duration}</p>
{#each movie.content.fonctions as f}
<p>{f.content.libelle}</p>
{/each}
{/each}

récupér paire name et value d'un objet
{#each Object.entries(data.content.movies) as [key, value]}
<p>{key} : {value.title}</p>
{/each}
