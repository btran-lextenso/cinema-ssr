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
        <li>
          <div class="photo">
            <a href="/stars-du-cinema/{person.path}">
              {#if i < 5}
                <img src="https://ba-api.lpnt.fr/images/personne/{person.img}" alt="" />
              {:else}
                <Lazy height={300}>
                  <img src="https://ba-api.lpnt.fr/images/personne/{person.img}" alt="" />
                </Lazy>
              {/if}
            </a>
          </div>
          <h2 class="fullname"><a href="/stars-du-cinema/{person.path}">{person.fullname}</a></h2>
          <small class="date_naissance">{person.date_naissance.replace(/(\d{4})-(\d{2})-(\d{2})/, '$3/$2/$1')}</small>
          <p class="profession">{person.profession}</p>
        </li>
    {/each}
  </ul>
{:catch error}
  <p class="font-red">Autre problème lors du chargement : {error}</p>
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
      document.getElementById("main").innerHTML = `<p class="msg_error_flux">Problème lors du chargement des données. Envoyez ce message à l'administrateur : <br /> ${window.location.href}<br /> erreur code ${response.status}</p>`;
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
