<template>
  <main>
    <List 
      v-if="list.show"
      :unpopularCharacter="list.unpopularCharacter"
      :labels="list.labels"
    />
    <BarChart :characters="barChart.characters"/>
  </main>

</template>

<script setup>
  import { computed, reactive } from 'vue'
  import List from './components/List.vue'
  import BarChart from './components/BarChart.vue';

  const baseURL = "https://rickandmortyapi.com/api"

  let characters = reactive({
    data: [],
  });

  let list = reactive({
      show: false,
      unpopularCharacter: [],
      labels: ["Character name", "Origin name","Origin dimension","Poplurity"],
  });

  let barChart = reactive({
    show: false,
    characters: []      
  });

  function setUnpopularCharacter(data) {
      const selectedOrigin = "Earth (C-137)";
      const filterByOrigin = data.filter(({ origin, location }) => origin.name.includes(selectedOrigin) && location.name.includes(selectedOrigin));
      // const filterByOrigin = data.filter( ({origin}) => origin.name.includes(selectedOrigin) );
      const lowestEpisodes = Math.min(...filterByOrigin.map(({episode}) => episode.length));
      const unpopularCharacter = filterByOrigin.filter( ({episode}) => episode.length === lowestEpisodes )
      console.log('unpopularCharacter', unpopularCharacter)
      list.unpopularCharacter = unpopularCharacter[0];
      list.show = true;
  }

  function setPopularity(allCharacters) {
    //console.log(allCharacters)
    const characters = [
      {
        name: "Rick Sanchez",
        legendColor: "#00bcc5",
      },
      {
        name: "Summer Smith",
        legendColor: "#ff9800",
      },
      {
        name: "Morty Smith",
        legendColor: "#f05b4f",
      },
      {
        name: "Beth Smith",
        legendColor: "#F06292",
      },
      {
        name: "Jerry Smith",
        legendColor: "#d70206",
      },
    ];
    let reduced = characters.reduce((filtered, item) => {

      const filterByName = allCharacters.filter( ({name}) => name === item.name );
      const highestEpisodes = Math.max(...filterByName.map(({episode}) => episode.length));

      let obj = { 
        name: item.name, 
        popularity: highestEpisodes,
        legendColor: item.legendColor
      }
      filtered.push(obj);
      return filtered;
    }, []);

    //console.log( reduced )
    barChart.characters = reduced;
  }

  function getAllCharacters(data) {
        let arr = [];
        let counter = 1;
        let total = data.info.count

        for(let i = 0; i < total; i++){
            arr.push(counter++);
            
            if( counter > total ){
                getData( `${baseURL}/character/[${arr}]`).then((data) => {
                    //console.log('all characters data', data)
                    characters.data = data;
                    setUnpopularCharacter(data);
                    setPopularity(data);
                }); 
            }
        }
    }

    async function getData(url) {
        try{
            const data = await fetch(url);
            const res = await data.json();
            return res;
        }catch(err){
            console.error(err)
        }
    };

    function initCharacters(url) {
        getData(url).then((data) => {
            //console.log('data', data)
            getAllCharacters(data)
        }); 
    }

    initCharacters(`${baseURL}/character/`)  
</script>

<style lang="scss">
  @import "/src/assets/css/reset.css";
  @import "/src/assets/scss/global.scss";
  @import "/src/assets/scss/variables.scss";
  @import "https://fonts.googleapis.com/css2?family=Lato:wght@400;700&display=swap";

  body{
    background: #fff;
  }

  .loader {
    position: fixed;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
    border: 5px solid $primary-color;
    -webkit-animation: spin 1s linear infinite;
    animation: spin 1s linear infinite;
    border-top: 5px solid $orange-color;
    border-radius: 50%;
    width: 50px;
    height: 50px;
  }

  @keyframes spin {
      0% { transform: rotate(0deg); }
      100% { transform: rotate(360deg); }
  }

  h2{
      text-transform: capitalize;
      font-size: 40px;
      font-weight: 700;
      margin: 40px auto;
      text-align: center;
      color: $orange-color;
      text-shadow: 1px 1px rgb(32 35 41 / 80%);
  }

  #app {
    color: $primary-color;
    font-family: $primary-font;
  }

  @media (max-width: 640px) {
    h2{
        font-size: 24px;
      }
    }    
</style>