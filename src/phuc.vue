<script setup>


import { ref } from 'vue'


function fetchPromise(URL) {
    return new Promise(function (resolve) {
        fetch(URL)
            .then(function (response) {
                const promise = response.json();
                resolve(promise);
            })
            .catch(function (error) {
                console.log('error!');
            });
    });
}





const html_out = ref('');





async function main() {
    let pokemons = []; 
    const data = await fetchPromise("https://pokeapi.co/api/v2/pokemon/?offset=0&limit=898");
    pokemons = data.results;
    const pokemonPromises = pokemons.map(pokemon => fetchPromise(pokemon.url));

    const pokemon__element = await Promise.all(pokemonPromises);    


    const pokemonList = document.querySelector(".pokemon__list");
    const button__pokemon = document.querySelector("button");



    let size = 20;
    function render__of__pokemon(dataPokemon,size,pokemon__element,element__id) {
        let imgs = "";
        button__pokemon.style.display = "flex";
        pokemonList.innerHTML = imgs;
        for (let i = 0; i < size; i++) {
            if (!element__id[0]) 
                element__id[i]=i;
            let element = "";
            if (!pokemon__element[i]) {
                button__pokemon.style.display = "none";
                break;
            }
            for (let j = 0;j < pokemon__element[i].types.length; j++)
                element+=`<div class="${pokemon__element[i].types[j].type.name}">
                              ${pokemon__element[i].types[j].type.name}
                         </div>`;
            imgs += `
            <div class="pokemon">
                <div class="id">#${element__id[i] + 1}</div>
                <img src="https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${element__id[i] + 1}.png" alt="">
                <div class="element__pokemon">
                     ${element} 
                </div>
                <div class="name">${dataPokemon[i].name}</div>
            </div>`
        }
        if (!pokemon__element[size]) {
                button__pokemon.style.display = "none";
            }






            html_out.value = imgs;





        
    }
    let element__id = [];
    render__of__pokemon(pokemons,size,pokemon__element,element__id);


    const name__inp = document.querySelector("input");
    function check(a,b){
        return b.includes(a);
    }
    let tmp = pokemons;
    let element__tmp = pokemon__element;
    name__inp.addEventListener('input',function(){
        let a = name__inp.value;
        let tmp = [];
        let element__tmp = [];
        let element__id = [];
        size = 5;
        console.log(a);
        for (let i = 0; i < pokemons.length; i++)
        {
            let b = pokemons[i].name;
            if (check(a, b)){
                tmp.push(pokemons[i]);
                element__tmp.push(pokemon__element[i]); 
                element__id.push(i);
            }
        }
        render__of__pokemon(tmp,size,element__tmp,element__id);
    })
    
    button__pokemon.addEventListener('click',function(){
        if (size < element__tmp.length)
            size = size + 5;
        else size = element__tmp.length;
        render__of__pokemon(tmp,size,element__tmp,element__id);
    })
}

main(); 


</script>

<template>
  <div class="container">
    <div class="header">
          API Pokemon
      </div>
      <div class="header">
          <input type="text" class="search" placeholder="Tìm kiếm một số Pokemon...">
      </div>
      <div class="formsize">



      <div class="pokemon__list" v-html="html_out">
      </div>




      
      </div>
      <div class="loadmore__button">
          <button>
              Load More
          </button>
      </div>
  </div>
</template>

<style scoped>
.container{
  width: 80vw;
  height: auto;
}
.header{
  display: flex;;
  width: 100%;
  height: 150px;
  font-size: 50px;
  align-items: center;
  justify-content: center;
}
input{
  font-size: 16px;;
  width: 80%;
  padding:20px;
  border-radius: 30px;
  align-items: center;
  justify-content: center;
}
.formsize{
  display: flex;
  width: 100%;
  justify-content: center;
  align-items: center;
}
.pokemon__list {
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
  align-items: center;
  justify-content: center;
}

.pokemon {
  text-align: center;
  min-width: 180px;
  min-height: 180px;
  max-width: 300px;
  max-height: 300px;
  background-color: white;
  border: 1px solid #ddd;
  border-radius: 10px;
  margin: 0%;
  transition: all ease 0.2s;
  flex-direction: column;
}
.pokemon__list >  :hover {
box-shadow: 5px 10px 18px #888;
}
img {
width: 100%;
height: 70%;
object-fit: cover;
}
.id{
width: 100%;
height: 10%;
text-align: center;
font-size: 30px;
font-weight: 20px;
}
.name{
width: 100%;
text-align: center;
height: 10%;
font-size: 30px;
font-weight: 20px;
}
.loadmore__button{
width: 100%;
display: flex;
justify-content: center;
align-items: center;
}
button{
display: flex;
width: 130px;
height: 60px;
margin: 5px;
justify-content: center;
align-items: center;
color: #fff;
border-radius: 30px;
background-color: red;
}

.element__pokemon{
  height: 10%;
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: center;
  gap :5px;
  padding: 3px;
}
.fire{
  background-color: #FF421C;
}
.grass{
  background-color: #78CD54;
}
.poison{
  background-color: #A33EA1;
}
.flying{
  background-color: #A98FF3;
}
.water{
  background-color: #6390F0;
}
.bug{
  background-color: #A6B91A;
}
.normal{
  background-color: #A8A77A;
}
.electric{
  background-color: #F7D02C;
}
.ground{
  background-color: #E2BF65;
}
.fairy{
  background-color: #D685AD;
}
.fighting{
  background-color: #C22E28;
}
.psychic{
  background-color: #F95587;
}
.rock{
  background-color: #B6A136;
}
.ice{
  background-color: #96D9D6;
}
.ghost{
  background-color: #735797;
}
</style>
