---
layout: post
title:  "Anime Database"
author: Aayush
categories: [ Jekyll ]
image: assets/images/2.jpg
---

<nav class="nav-wrapper navbar-fixed black">
  <header class="center brand-logo">
    <h1 class="brand">Anime Search Database</h1>
  </header>
</nav>
<main class="row">
  <section class="col s12">
    <form id="anime-search" class="row">
      <fieldset class="input-field col s12">
        <i class="material-icons prefix">search</i>
        <input type="search" name="anime" id="anime" placeholder="enter a anime title" autocomplete="off" required />
      </fieldset>
      <fieldset class="col s12">
        <button class="btn waves-effect black full-btn" type="submit">
          Search here
        </button>
      </fieldset>
    </form>
  </section>
  <section class="col s12 table-fixed area scroll">
    <table class="striped centered">
      <thead class="black white-text">
        <tr>
          <th>Poster</th>
          <th>Title</th>
          <th>Synopsis</th>
          <th>Type</th>
          <th>Episodes</th>
          <th>Anime debut</th>
          <th>Anime Status</th>
          <th>Rated</th>
        </tr>
      </thead>
      <!-- render DOM tech stack -->
      <tbody></tbody>
    </table>
  </section>
</main>
<!-- partial -->
  <script src='https://cdnjs.cloudflare.com/ajax/libs/axios/0.21.1/axios.min.js'></script>
<script src='https://cdnjs.cloudflare.com/ajax/libs/materialize/1.0.0/js/materialize.min.js'></script>
<script src='https://cdnjs.cloudflare.com/ajax/libs/timeago.js/4.0.2/timeago.min.js'></script>
  <script>
  const { toast } = M;
const { format } = timeago

// DOM elements
const animeList = document.querySelector('tbody')
const search = document.getElementById('anime')
const sendSearch = document.getElementById('anime-search')

// DOM fragment
const fragment = document.createDocumentFragment()

// anime search function
async function animeTool(query) {
  try {
    // call api
    const res = await axios.get("https://api.jikan.moe/v3/search/anime", {
      params: {
        q: query
      }
    })

    res.data.results.map((animeData) => {
      // row element
      const row = document.createElement('tr')

      // column elements
      const animePoster = document.createElement('td')
      const animeTitle = document.createElement('td')
      const animeSynopsis = document.createElement('td')
      const animeType = document.createElement('td')
      const animeEpisodes = document.createElement('td')
      const animeDebut = document.createElement('td')
      const animeFinish = document.createElement('td')
      const animeRated = document.createElement('td')
      
      // image
      const poster = document.createElement('img')

      animeTitle.textContent = animeData.title
      animeSynopsis.textContent = animeData.synopsis
      animeType.textContent = animeData.type
      animeEpisodes.textContent = animeData.episodes
      animeDebut.textContent = format(animeData.start_date)
      
      if (animeData.end_date === null) {
        animeFinish.textContent = 'Current Date'
      } else {
        animeFinish.textContent = format(animeData.end_date)
      }
      
      animeRated.textContent = animeData.rated

      poster.src = animeData.image_url
      poster.alt = `poster ${animeData.title}`
      poster.classList.add('poster')
      
      animePoster.appendChild(poster)

      row.appendChild(animePoster)
      row.appendChild(animeTitle)
      row.appendChild(animeSynopsis)
      row.appendChild(animeType)
      row.appendChild(animeEpisodes)
      row.appendChild(animeDebut)
      row.appendChild(animeFinish)
      row.appendChild(animeRated)

      fragment.appendChild(row)

      animeList.appendChild(fragment)
    })

  } catch (err) {
    toast({ html: err.message })
    console.error(err.message)
  }
}

// events
sendSearch.addEventListener('submit', (e) => {
  animeList.innerHTML = ''

  animeTool(search.value)

  e.preventDefault()
  sendSearch.reset()
})

document.addEventListener('keypress', (e) => {
  if (e.keyCode === 13) {
    e.preventDefault()
    return false
  }
})
  </script>

<style>    
  /* id */
.brand {
  text-transform: capitalize;
  margin: 5% 0;
  font-size: 26pt;
}
/* fallback */
.col.s12 .full-btn {
  background-color: #4CAF50; /* Green */
  border: none;
  color: white;
}
.poster {
  width: 50px;
  height: 100px;
}
/* table */
.table-fixed {
  scrollbar-width: thin;
  scrollbar-color: #fff #000;
  overflow: auto;
  height: auto;
  width: auto;
  border-collapse: collapse;
}
.table-fixed thead th {
  position: sticky;
  background: black;
  top: 0;
}   
</style>
