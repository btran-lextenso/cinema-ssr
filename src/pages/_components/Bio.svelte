<script>
export let photo
export let nom
export let profession
export let date_naissance
export let lieu_naissance
export let nationalite
export let commentaire
export let url_dbpedia
</script>

<div class="row star ma20">
  <img src="https://ba-api.lpnt.fr/images/personne/{photo}" alt="" />
  <div class="infos1 ma20">
    <h1>{nom}</h1>
    <p class="profession"> <b>Profession : </b>{profession}</p>
    <p class="date_naissance"><b>Date de naissance : </b> {date_naissance}</p>
    <p class="lieu_naissance"><b>Lieu de naissance : </b> {lieu_naissance}</p>
    <p class="nationalite"><b>Nationalité : </b> {nationalite}</p>
    <p class="bio"><b>Mini-bio : </b> {commentaire}</p>
    <p class="small"><a href={url_dbpedia}>DBPedia</a></p>
  </div>
</div>
