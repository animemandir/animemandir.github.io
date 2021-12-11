---
layout: post
title:  "Anime Database"
author: Aayush
categories: [ Jekyll ]
image: assets/images/1.jpg
---


<nav class="nav-wrapper navbar-fixed black">
  <header class="center brand-logo">
    <h1 class="brand">Manga Search Database</h1>
  </header>
</nav>
<main class="row">
  <section class="col s12">
    <form id="manga-search" class="row">
      <fieldset class="input-field col s12">
        <i class="material-icons prefix">search</i>
        <input type="search" name="manga" id="manga" placeholder="enter a manga title" autocomplete="off" required />
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
          <th>News</th>
          <th>Type</th>
          <th>Pictures</th>
          <th>More Info</th>
          <th>Status/Shedule</th>
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
const mangaList = document.querySelector('tbody')
const search = document.getElementById('manga')
const sendSearch = document.getElementById('manga-search')

// DOM fragment
const fragment = document.createDocumentFragment()

// manga search function
async function mangaTool(query) {
  try {
    // call api
    const res = await axios.get("https://api.jikan.moe/v3/search/manga", {
      params: {
        q: query
      }
    })

    res.data.results.map((mangaData) => {
      // row element
      const row = document.createElement('tr')

      // column elements
      const mangaPoster = document.createElement('td')
      const mangaTitle = document.createElement('td')
      const mangaNews = document.createElement('td')
      const mangaType = document.createElement('td')
      const mangaChapters = document.createElement('td')
      const mangaScore = document.createElement('td')
      const mangaFinish = document.createElement('td')
      const mangaRated = document.createElement('td')
      
      // image
      const poster = document.createElement('img')

      mangaTitle.textContent = mangaData.title
      mangaNews.textContent = mangaData.news
      mangaType.textContent = mangaData.type
      mangaChapters.textContent = mangaData.chapters
      mangaScore.textContent = format(mangaData.score)
      
      if (mangaData.end_date === null) {
        mangaFinish.textContent = 'Current Date'
      } else {
        mangaFinish.textContent = format(mangaData.end_date)
      }
      
      mangaRated.textContent = mangaData.rated

      poster.src = mangaData.image_url
      poster.alt = `poster ${mangaData.title}`
      poster.classList.add('poster')
      
      mangaPoster.appendChild(poster)

      row.appendChild(mangaPoster)
      row.appendChild(mangaTitle)
      row.appendChild(mangaNews)
      row.appendChild(mangaType)
      row.appendChild(mangaChapters)
      row.appendChild(mangaScore)
      row.appendChild(mangaFinish)
      row.appendChild(mangaRated)

      fragment.appendChild(row)

      mangaList.appendChild(fragment)
    })

  } catch (err) {
    toast({ html: err.message })
    console.error(err.message)
  }
}

// events
sendSearch.addEventListener('submit', (e) => {
  mangaList.innerHTML = ''

  mangaTool(search.value)

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
