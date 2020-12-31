<template>
  <div >
  <div class="list"  v-if="state == 'list'">
   <ul >
      <li v-for="(match, index) in matches" :key="index">
        <a @click="selectMatch(match)">
          <div class="card">
            <h2>{{ match.title }}</h2>
            <img :src="match.thumbnail" alt="" />
          </div>
        </a>
      </li>
    </ul>
  </div>
   
    <div v-if="state == 'video'" class="content-video">
        <div v-html="embed" class="video"></div>
    </div>
  </div>
</template>
<script>
export default {
  name: "ListMatches",
  props: {
    state: String,
    matches: Array,
  },
  data() {
    return {
      state: null,
      embed: null
    };
  },
  methods:{
      selectMatch(match){
          this.state = 'video'
          this.embed = match['embed']
      console.log(this.embed, "🚀 ~ file: ListMatches.vue ~ line 29 ~ selectMatch ~ match", this.state)
          
      }
  }
};
</script>

<style  scoped>
ul {
  list-style: none;
  display: flex;
  justify-content: center;
  flex-direction: column;
      padding: 0;
}
a {
  display: flex;
  justify-content: center;
}
.card {
  width: 100%;
}
.card:hover {
  background-color: #eaeaee;
  width: 40%;
  padding: 10px 0 50px 0;
  cursor: pointer;
}
img{
    max-width: 480px;
    max-height: 360px;
    width: 100%;
}
.video{
    max-width: 480px;
    max-height: 360px;
    width: 100%;
}
.content-video{
    display: flex;
    justify-content: center;
    padding: 40px 0;
}
</style>


