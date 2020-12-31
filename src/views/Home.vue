<template>
  <div class="home">
  <div class="content-filters">
    <div class="filters">
        <!--Select leagues to show matches-->
        <select name="" id="" @change="setLeague">
          <option :value="null"  selected>LIGAS</option>
          <option v-for="(league, index) in leagesNames" :key="index" :value="league" >
            {{league}}
          </option>
        </select>
<br><br>
      <!--Select teams to show matches-->
        <select name="" id="" @change="setTeam">
          <option :value="null"   selected>EQUIPOS</option>
          <option v-for="(team, index) in teamsNames" :key="index" :value="team" >
            {{team}}
          </option>
        </select>
     </div>
  </div>



    <!--List Matches-->
    <ListMatches :matches="matchesRandom" :state="'list'" v-if="!currentLeague && !currentTeam" /> 
    <ListMatches :matches="leagues[currentLeague]" :state="'list'" v-if="currentLeague && !currentTeam" />
    <ListMatches :matches="teams[currentTeam]" :state="'list'" v-if="currentTeam" />
  </div>
</template>

<script>
// @ is an alias to /src
import HelloWorld from '@/components/HelloWorld.vue'
import ListMatches from '@/components/ListMatches.vue'
export default {
  name: 'Home',
   components: {
    HelloWorld,
    ListMatches
   
  },
  data(){
    return {
      matches: null,
      matchesRandom: [],
      leagues: [],
      leagesNames: [],
      currentLeague: null,
      currentTeam: null,
      teams: [],
      teamsNames:[]
    }
  },
  beforeCreate(){
     this.axios
      .get('https://www.scorebat.com/video-api/v1/')
      .then(response => {
        this.matches = response['data']
        this.matchesRandom =  this.generateRandomMatches(this.matches, 5);
        this.leagues = this.generateLeagues(this.matches);
        this.leagesNames = Object.keys(this.leagues);
        this.teams =  this.generateTeams(this.matches);  
        this.teamsNames =  Object.keys(this.teams);
        console.log("🚀 ~ file: Home.vue ~ line 46 ~ beforeCreate ~ this.teamsNames", this.teamsNames)
        
      }).catch(err=>{
        alert('se ha presentado un error al cargar la pagina')
      })
  },
  methods:{
    between(min, max) {  
      return Math.floor(
        Math.random() * (max - min) + min
      )
    },
    generateRandomMatches(matches, limit){
      let result = [];
      for (let index = 0; index < limit; index++) {
          let rangeNumber =  this.between(1, matches.length)
            result[index] = matches[rangeNumber];      
       }
       return result;
    },
     generateLeagues(matches){
       let obj = {};
       this.matches.map((match)=>{
          let nameLeague = match['competition']['name'];
          obj[nameLeague] = obj[nameLeague] || [];
          obj[nameLeague].push(match);
       })
       return obj;
    },
     generateTeams(matches){
       let obj = {};
       this.matches.map((match)=>{
          let side1 = match['side1']['name'];
          let side2 = match['side2']['name'];
          obj[side1] = obj[side1] || [];
          obj[side1].push(match);
          obj[side2] = obj[side2] || [];
          obj[side2].push(match);
       })
       return obj;
    },
    setLeague(nameLeague){
      this.currenTeam = null;
      this.currentLeague = nameLeague.target.value;    
    },
    setTeam(nameLeague){
      this.currentTeam = nameLeague.target.value;    
    }
  }
}
</script>
<style  scoped>
  .content-filters{
    display: flex;
    justify-content: center;
  }
  select{
    max-width: 480px;
    width: 100%;
    height: 40%;
  }
</style>
