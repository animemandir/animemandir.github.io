---
layout: post
title:  "Anime Database"
author: Aayush
categories: [ Jekyll ]
image: assets/images/6.jpg
---

<title>Pokedex Pokeapi | Anime Mandir</title>
  <link rel='stylesheet' href='https://cdn.jsdelivr.net/npm/bootstrap@5.0.1/dist/css/bootstrap.min.css'>
<link rel='stylesheet' href='https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.8.2/css/all.min.css'>
<link rel='stylesheet' href='https://cdn.datatables.net/1.10.25/css/jquery.dataTables.min.css'>

<div id="page">
  <section id="main">
    <div class="container">
      <div class="row ">
        <div class="col-lg-12">
          <div class="poke-container-search">
            <img defer src="https://upload.wikimedia.org/wikipedia/commons/9/98/International_Pok%C3%A9mon_logo.svg" alt="POKEMON Logo Pokedex api" id="pokemon-logo"/>
            
            
            <div class="searchBar">
              <form action="">
                <label for="pokemon">PokeSearch:</label>
                <div class="search-wrap">
                  <input type="text" name="pokemon" id="search" placeholder="Ex: Charizard or 6" aria-label="Buscar" required>
                  <button type="submit" aria-label="Buscar...">
                    <i class="fab fa-searchengin"></i>
                  </button>
                </div>
              </form>
            </div>
          </div>
        </div>
        <div class="col-lg-12">
          <div class="poke-container result">
            <h1 class="main-title">INFO-</h1>
            <div class="lds-dual-ring"></div>
            <div id="pokeOutput">
              <div class="top-container">
                <div class="image-container">
                  <div id="pic-nav">
                    <button type="button" name="left" id="leftBtn" class="changeBtn" aria-label="Girar izquierda"><i class="fas fa-angle-left"></i></button><button type="button" name="default" id="defaultBtn" class="bigBtn" >Default</button><button type="button" name="Shiny"
          id="shinyBtn" class="bigBtn">Shiny</button><button type="button" name="left" id="rightBtn" class="changeBtn" aria-label="Girar derecha"><i class="fas fa-angle-right"></i></button>
                  </div>
                  <div class="poke-name-num">
                    <span class="name poke-info"></span>
                    <span class="idNum poke-info"></span>
                  </div>
                  <div class="size">
                    <span class="poke-info"><span class="weight"></span></span>
                    <span class="poke-info"><span class="height"></span></span>
                  </div>
                  <img src="" alt="" id="pokeImage">
                  <div id="types"></div>
                </div>
              </div>
              <div class="bottom-container">
                <table>
                  <tr>
                    <th>Base</th>
                    <th>Stats</th>
                  </tr>
                  <tr>
                    <td>HP:</td>
                    <td class="hp"></td>
                  </tr>
                  <tr>
                    <td>Attack:</td>
                    <td class="attack"></td>
                  </tr>
                  <tr>
                    <td>Defense:</td>
                    <td class="defense"></td>
                  </tr>
                  <tr>
                    <td>Sp.Attack:</td>
                    <td class="special-attack"></td>
                  </tr>
                  <tr>
                    <td>Sp.Defense:</td>
                    <td class="special-defense"></td>
                  </tr>
                  <tr>
                    <td>Speed:</td>
                    <td class="speed"></td>
                  </tr>
                </table>
                <span class="gameVersion"><p>Pokemon Versions</p><span></span></span>
                <div id="PokeMoves">
                  <table id="move" class="" style="width:100%">
                    <thead>
                      <tr>
                        <td>Move</td>
                        <td>Version Name</td>
                        <td>Method Learn</td>
                        <td>Level Learned</td>
                      </tr>
                    </thead>
                    <tbody></tbody>
                  </table>
                </div>
              </div>
            </div>
            <p class="msgerror alert alert-danger"></p>
          </div>
          <footer>
            <small align="right">Powered by <a>PokéAPI</a></small>
          </footer>
          <div id="testing"><div>
            </div>
          </div>
        </div> 
        </section>
    </div>
<!-- partial -->
  <script src='https://code.jquery.com/jquery-3.5.1.js'></script>
<script src='https://cdn.datatables.net/1.10.25/js/jquery.dataTables.min.js'></script>
    <script  type="text/javascript">
    //////jquery - script
(function($) {
  $(document).ready(function() {
    $("form").on("submit", function(e) {
      $('#pokeOutput').hide();
      $('.msgerror').html('').hide();
      $('.lds-dual-ring').show();
      e.preventDefault();
      var userInput = $("#search").val().toLowerCase().replace(/[^a-zA-Z0-9]/g, '');
      $("#search").val(userInput);
      var url = "https://pokeapi.co/api/v2/pokemon/" + userInput;
      //console.log(url);
      $.ajax({
        url: url,
        dataType: "json",
        method: "GET",
        success: function(data) {
          let name = data.forms[0].name,
              pokeImgFront = data.sprites.front_default,
              pokeImgBack = data.sprites.back_default,
              pokeImgShinyFront = data.sprites.front_shiny,
              pokeImgShinyBack = data.sprites.back_shiny,
              shiny = false,
              frontImg = true,
              speed = data.stats[0].base_stat,
              spDef = data.stats[1].base_stat,
              spAtk = data.stats[2].base_stat,
              def = data.stats[3].base_stat,
              atk = data.stats[4].base_stat,
              hp = data.stats[5].base_stat,
              id = "#" + data.id,
              weight = "<span class='stat'>Weight: </span>" + data.weight,
              height = "<span class='stat'>Height: </span>" + data.height,
              game_indices = [],
              types = [],
              moves = [],
              version_group_details = [],
              version_group = [],
              move_learn_method = [],
              version_Name = [];
          for (let i = 0; i < data.types.length; i++) {
            let type = data.types[i].type.name;
            types.push(type);
          }
          for(let i = 0; i < data.game_indices.length; i++){
            let version = data.game_indices[i].version.name;
            game_indices.push(version);
          }
          for(let i = 0; i < data.moves.length; i++ ){
            let move = data.moves[i].move;
            let moveName = move.name;  
            moves.push(moveName);
          }
          function pokemonType(types) {
            $("#types").html("");
            for (let i = 0; i < types.length; i++) {
              $("#types").append(
                "<div class='pokeType poke-info " +
                types[i] +
                "'>" +
                types[i] +
                " </div>"
              );
            }
          }
          function pokemonVersions(game_indices) {
            $(".gameVersion span").html("");
            for (var i = 0; i < game_indices.length; i++) {
              $(".gameVersion span").append(
                "<a href='javascript:void(0);' class='" + game_indices[i] + "'>" + game_indices[i].replace(/-/g,' ') + "</a>"
              );
            }
          }
          function pokemonMove(moves, version_Name) {
            var dataSet = [];
            for (let i = 0; i < moves.length; i++) {
              let version_groups =  data.moves[i].version_group_details;     
              for (let e = 0; e < data.moves[i].version_group_details.length; e++) {
                let version_Name = version_groups[e].version_group.name;
                let method_Learn = version_groups[e].move_learn_method.name;
                let level_learned_at = version_groups[e].level_learned_at;
                dataSet.push([moves[i].replace(/-/g,' '),version_Name.replace(/-/g,' '),method_Learn.replace(/-/g,' '),level_learned_at])
              }   
            }
            var moveTable = $('#move').DataTable( {
              destroy: true,
              data: dataSet,
              columns: [
                { title: "Move" },
                { title: "Version Name" },
                { title: "Method Learn" },
                { title: "Level Learned" }
              ]
            } );
            $(".gameVersion a").on("click",function() {
              var VersionToSearch = this.innerText;
              moveTable.search(VersionToSearch).draw();
            });
          }	
          $("#defaultBtn").click(function() {
            $("#pokeImage").attr("src", pokeImgFront);
            shiny = false;
            frontImg = true;
          });
          $("#shinyBtn").click(function() {
            $("#pokeImage").attr("src", pokeImgShinyFront);
            shiny = true;
            frontImg = true;
          });
          $(".changeBtn").click(function() {
            if (shiny == false && frontImg == true) {
              shiny = false;
              frontImg = false;
              $("#pokeImage").attr("src", pokeImgBack);
            } else if (shiny == false && frontImg == false) {
              shiny = false;
              frontImg = true;
              $("#pokeImage").attr("src", pokeImgFront);
            } else if (shiny == true && frontImg == true) {
              shiny = true;
              frontImg = false;
              $("#pokeImage").attr("src", pokeImgShinyBack);
            } else if (shiny == true && frontImg == false) {
              shiny = true;
              frontImg = true;
              $("#pokeImage").attr("src", pokeImgShinyFront);
            }
          });
          $(".name").html(name);
          $(".idNum").html(id);
          $("#pokeImage").attr("src", pokeImgFront);
          $(".hp").html(hp);
          $(".attack").html(atk);
          $(".defense").html(def);
          $(".special-attack").html(spAtk);
          $(".special-defense").html(spDef);
          $(".speed").html(speed);
          $(".weight").html(weight);
          $(".height").html(height);
          pokemonType(types);
          pokemonVersions(game_indices);
          pokemonMove(moves, version_Name);
          if($('#pokeOutput').css('display') == 'none'){
            $('#pokeOutput').fadeIn('slow');
          }
          $('.lds-dual-ring').hide();
        },//SUCCESS
        error: function(xhr, status, error) {
          $('.lds-dual-ring').hide();
          $('.msgerror').html(status +'<br>' + xhr.responseText);
          $('.msgerror').show();
        } //ERROR
      }); //AJAX
    }); //FORM
  });//END DOM READY
})(jQuery);
    </script>
	
	<style>
    #pokeOutput,
.lds-dual-ring{
  display:none;
}
#page{
  
  overflow:hidden;
}

body {
  margin: 0;
  display: flex;
  justify-content: center;
  
  font-family: "Work Sans", sans-serif;
}
section{
  width: auto;
}
.poke-container-search{
  text-align:center;
}
.poke-container {
  background: rgba(0,0,0,.7);
  width: auto;
  border: 10px solid #88061c;
  display: -ms-flexbox;
  display: -webkit-flex;
  display: flex;
  -webkit-flex-direction: column;
  -ms-flex-direction: column;
  flex-direction: column;
  -webkit-flex-wrap: wrap;
  -ms-flex-wrap: wrap;
  flex-wrap: wrap;
  -webkit-justify-content: center;
  -ms-flex-pack: center;
  justify-content: center;
  -webkit-align-content: center;
  -ms-flex-line-pack: center;
  align-content: center;
  -webkit-align-items: center;
  -ms-flex-align: center;
  align-items: center;
}
@media (max-width: auto){
  .container-left,
  .container-right {
    width:auto;
  }
  [class^="col-"], [class*=" col-"] {
    padding: 0;
  }
}

.searchBar {
  padding: 15px 0;
  text-align: left;
  background-color: #ec3f3f;
  border-radius: 4px 4px 0 0;
}

label {
  color:#050606;
  display:block;
  text-align:left;
  padding:5px;
  font-size: 1.3rem;
}

input[type="text"] {
  height: 40px;
  padding-left: 10px;
  display: inline-block;
  vertical-align:middle;
}
.search-wrap{
  display:inline;
  display: block;
}
#pokeOutput {
  width: 90%;
  padding: 10px;
}
#pokemon-logo{
  max-width:200px;
  margin:0 auto;
}
#pokeImage {
  display: block;
  margin: 10px auto;
  width: 350px;
  position: relative;
}

.base-title {
  font-size: 1.3rem;
  font-weight: 700;
}

.stat {
  font-weight: 700;
}

.poke-name-num {
  padding:10px;
}

.size {
  padding:10px;
}

.name {
  display: block;
  text-transform: capitalize;
  font-size: 1.6rem;
}

.poke-info {
  display: inline-block;
  margin-right: 2px;
}

.fa-searchengin {
  margin: 0;
  padding: 0;
  font-size: 35px;

}

.image-container {
  white-space: nowrap;
  border: 1px solid #e8332a;
  color: #fff;
  font-size: 1.2rem;
}
.top-container,
.bottom-container{
  background-color: rgba(255,255,255,0.2);
}
#page{
  background: url('https://assets.pokemon.com//assets/cms2-es-es/img/misc/virtual-backgrounds/pokemon/detective-pikachu.jpg');
  background-size: cover;
  background-repeat: no-repeat;
  background-position: center;
}
#types {
  padding:10px;

}
.search-wrap button{
  vertical-align:middle;
}
button {
  display: inline-block;
  margin: 0;
  font-size: 20px;
  font-weight: 200!important;
  background-color: #fff;
  color: #000;
  border: 1px solid #e8332a;
  position: relative;
  left: 0px;
  outline: none;
  cursor: pointer;
}

button:hover {
  box-shadow:0px 0px 0px 1px #e8332a inset;
}

button:active {
  background-color: #f4f1f1;
}

.bigBtn {
  height: 40px;
  width: 40%;
  font-weight: 700;
  position: relative;
  top: -2px;
}

.changeBtn {
  height: 39px;
  width: 10%;
  font-size: 1.5rem;
}

table {
  border-collapse: collapse;
  width: 100%;
  font-size: 1.2rem;
}

td,
th {
  color:#f1f6ff;
  border: 1px solid #e8332a;
  text-align: left;
  padding: 8px;
}
#move thead td{
  background-color: #000!important;
}
#move td,
#move th {
  color:#f1f6ff;
  border: 1px solid #e8332a;
  text-align: left;
  padding: 8px;
  text-transform:capitalize;
}
#move tr:nth-child(odd) {
  background-color: #3D3D45;
}
#move tr:nth-child(even) {
  background-color: #7b0b06!important;
}
tr:nth-child(even) {
  background-color: #7b0b06!important;
}
#move_length label,
#move_filter label,
#move_filter input,
#move_info,
.dataTables_wrapper .dataTables_paginate .paginate_button,
.dataTables_wrapper .dataTables_paginate .paginate_button.disabled,
.dataTables_wrapper .dataTables_length, .dataTables_wrapper .dataTables_filter, .dataTables_wrapper .dataTables_info, .dataTables_wrapper .dataTables_processing, .dataTables_wrapper .dataTables_paginate{
  color:#FFF!important;
}
#move_length select,
#move_length select option{
  color:#FFF!important;
  background-color: #3D3D45;
}
.pokeType {
  padding: 5px;
  color: #fff;
  border-radius: 5px;
  text-transform: capitalize;
  margin-left: 5px;
  font-weight: 700;
}
.normal {
  background-color: #a8a878;
}

.grass {
  background-color: #78c850;
}

.ground {
  background-color: #e0c068;
}

.fighting {
  background-color: #c03028;
}

.rock {
  background-color: #b8a038;
}

.steel {
  background-color: #b8b8d0;
}

.fire {
  background-color: #f08030;
}

.electric {
  background-color: #f8d030;
}

.flying {
  background-color: #a890f0;
}

.psychic {
  background-color: #f85888;
}

.bug {
  background-color: #a8b820;
}

.dragon {
  background-color: #7038f8;
}

.water {
  background-color: #6890f0;
}

.ice {
  background-color: #98d8d8;
}

.poison {
  background-color: #a040a0;
}

.dark {
  background-color: #705848;
}

.ghost {
  background-color: #705898;
}

.fairy {
  background-color: #ffaec9;
}
.red{
  background-color: red;
  color: white;
}
.blue{
  background-color: blue;
  color: white;
}
.yellow{
  background-color: yellow;
  color:black;
}
.green{
  background-color: #00fe00;
  color: white;
}
.ruby{
  background-color: #E0115F;
  color: white;
}
.sapphire{
  background-color: #082567;
  color: white;
}
.gold{
  background-color: #f5c21c;
  color: black;
}
.black{
  color: white;
  background-color: black;
}
.silver{
  background-color: #eaeaea;
  color:black;
}
.crystal{
  background-color: #9CA8D6;
  color:black;
}
.diamond{
  background-color: #ebfbfe;
  color:black;
}
.pearl{
  background-color: #FDEEF4;
  color:black;
}
.emerald{
  background-color: #50C878;
  color: white;
}
.firered{
  background-color: #E06531;
  color: white;
}
.platinum{
  background-color: #E5E4E2;
  color:black;
}
.leafgreen{
  background-color: #467C24;
  color: white;
}
.heartgold{
  background-color: #F6D12E;
  color:black;
}
.soulsilver{
  background-color: #AFB9C2;
  color:black;
}
.white{
  background-color: #FFFFFF;
  color:black;
}
.black-2{
  background-color: #1B1B1D;
  color: white;
}
.white-2{
  background-color:#f9f9f9;
  color:black;
}
.gameVersion a{
  padding: 0px 10px;
  display: inline-block;
  margin: 10px 5px;
  border-radius: 10px;
  text-transform: capitalize;
  cursor:pointer;
  text-decoration:none;
  border: 1px solid transparent;
}
.gameVersion a:hover,
.gameVersion a:focus{
  text-decoration: none;
  color: #ffffff;
  transition: .3s;
  background: transparent;
  border: 1px solid #fff;
}

.gameVersion p{
  margin: 5px auto 0px auto;
  padding: 0px 10px;
  text-align: center;
  color: #FFF;
}
footer {
  background-color: #e8332a;
  text-align: center;
  left: 0px;
  bottom: 0px;
  width: 100%;
  padding:10px 0px;
  color: #fff;
}

footer a {
  color: #fff;
  text-decoration: none;
}

footer a:hover {
  color: #ffcb08;
}
.msgerror{
  display:none;
  text-align:center;
  text-transform:capitalize;
  font-size:13px;
  margin:20px auto 25px auto;
  width:80%;
}
.lds-dual-ring {
  display: none;
  width: 80px;
  height: 80px;
}
.lds-dual-ring:after {
  content: " ";
  display: block;
  width: 64px;
  height: 64px;
  margin: 8px;
  border-radius: 50%;
  border: 6px solid #fff;
  border-color: #fff transparent #fff transparent;
  animation: lds-dual-ring 1.2s linear infinite;
}
@keyframes lds-dual-ring {
0% { transform: rotate(0deg); }
100% { transform: rotate(360deg); }}
</style>
