<div class="txtcenter search">
<form>
  <label for="profession">Rechercher la star </label>
  <select id="profession" bind:value={profession} on:change={()=> personsPromise = getPersons()}>
    <option value="home" selected>Tous</option>
    <option value="acteurs">Acteurs</option>
    <option value="directeurs">Directeurs</option>
    <option value="producteurs">Producteurs</option>
  </select>
  <input type="search" placeholder="nom ou prénom" bind:value="{stringToMatch}">
</form>
</div>
<p class="txtcenter ma20 search_choice">
  <span class="capitalize">{profession ==='home' ? 'Tous' : profession }</span> { stringToMatch === '' ? '' : "et "+stringToMatch}
</p>


{#await personsPromise}
<Loader />

{:then persons}
  <ul class="persons">
    {#each getFilteredPersons(persons, stringToMatch) as person, i}
      {#if i < 5}
        <li>
          <div class="photo">
              <a href="/stars-du-cinema/{person.path}"><img src="https://ba-api.lpnt.fr/images/personne/{person.img}" alt="" /></a>
          </div>
          <h2 class="fullname"><a href="/stars-du-cinema/{person.path}">{person.fullname}</a></h2>
          <small class="date_naissance">{person.date_naissance}</small>
          <p class="profession">{person.profession}</p>

        </li>

        {:else}
        <li>
          <div class="photo">
            <a href="/stars-du-cinema/{person.path}">
              <Lazy height={300}>
                <img src="https://ba-api.lpnt.fr/images/personne/{person.img}" alt="" />
              </Lazy>
            </a>
          </div>
          <h2 class="fullname"><a href="/stars-du-cinema/{person.path}">{person.fullname}</a></h2>
          <small class="date_naissance">{person.date_naissance}</small>
          <p class="profession">{person.profession}</p>
        </li>
      {/if}

    {/each}
  </ul>

<!-- {:catch error}
  <p class="font-red">Problème lors du chargement : {error}</p> -->

{/await}


<script>
  import { ready } from '@roxi/routify'
  import { metatags } from '@roxi/routify'
  import Loader from './_components/Loader.svelte'
  import Lazy from 'svelte-lazy'

  let persons = [],
  profession = 'home',
  stringToMatch = '',

  getPersons = async() => {
    let url = `https://www.devinez-le-film.fr/${profession}.json`;
    let response = await fetch(url);
    if (!response.ok) {
      document.getElementById("main").innerHTML = `<p class="msg_error_flux">Veuillez réessayer. Le flux de données répond avec le code erreur ${response.status}</p>`;
      return [];
    }
    let data = await response.json();
    //console.log(data.persons)
    $ready()
    return data.persons;
  }

  let personsPromise = getPersons();

  function getFilteredPersons(persons, stringToMatch){
    if(stringToMatch){
      return persons.filter(person => {
        return person.fullname.toLowerCase().includes(stringToMatch.toLowerCase());
      });
    } else {
      return persons
    }
  }

metatags.title = "Cinéma - page d'accueil"
metatags.description = 'Description liste des stars du cinéma'

</script>
