<template>
  <div class="catalog">
    <div class="filterWrapper">
      <input v-model="inputFilter"/>
      <button  @click="handleButtonClick">Filter</button>
    </div>
   <ul ref="listWrapper">
     <li v-for="(gif, index) in gifs" :key="index">
       <p>{{gif.title}}</p>
       <img :src="gif.img">
     </li>
   </ul>
   <div v-if="isLoading" class="loader">Loading</div>
  </div>
</template>

<script>
export default {
  name: 'GifCatalog',
  props: {
    msg: String
  },
  data() {
    return {
      inputFilter: "",
      gifs: [],
      totalGifs: 0,
      isLoading: false
    }
  },
  mounted() {
     window.addEventListener('scroll', (event) => {this.handleScrollLoad(event)});
  },
  methods:{
    async fetchData(filter) {
      try{
        this.isLoading = true;
        const consumeApi = await fetch(
        `https://api.giphy.com/v1/gifs/search?api_key=9u2Bd5KGWMJ4yvdopnqB7nXh5L4DXf6p&q=${filter}&&offset=${this.gifs.length}&&limit=10`);
        if(consumeApi.status === 200) {
          const data = await consumeApi.json();
          this.totalGifs = data.pagination.total_count;
          this.isLoading = false;
          this.simplifyData(data.data);
        }
      }catch(error){
        console.log(error);
      }
    },
    
    handleButtonClick() {
      this.gifs = [];
      this.fetchData(this.inputFilter);
    },

    simplifyData(data) {
      const array = [];
      data.forEach(element => {
        const formatElement = {
          id: element.id,
          title: element.title,
          img: element.images.original.url,
        };
        array.push(formatElement);
      });
      if(this.gifs.length){
        this.gifs = this.gifs.concat(array);
      }else{
        this.gifs = array;
      }
    },

    handleScrollLoad(e) {
      e.preventDefault();
      if (this.gifs.length === this.totalGifs || this.isLoading) return;
      const scroll = document.documentElement.scrollTop;
      const windowHeight = window.innerHeight;
      if (scroll + windowHeight >= document.documentElement.offsetHeight) {
        this.fetchData(this.inputFilter)
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

.catalog {
  display: flex;
  flex-direction: column;
  align-items: center;
}
.filterWrapper{
  padding: 15px;
  display: flex;
  width: 100%;
  max-width: 400px;
  justify-content: space-around;
  justify-self: center;
  gap: 15px;
  flex-shrink: 1;
}
.loader{
  position: fixed;
  bottom: 0;
  width: 100%;
  background-color: #70acf0;
  color: #fff;
  font-weight: 600;
  padding: 15px;
  
}
input{
  width: 100%;
  outline: none;
  border-radius: 5px;
  border: 2px solid rgb(218, 218, 218);
}
button{
  background-color: #70acf0;
  color: #fff;
  outline: none;
  border: none;
  border-radius: 5px;
  padding: 10px;
  cursor: pointer;
  flex-grow: 1;
}
ul {
  position: relative;
  list-style-type: none;
  padding: 0;
  display: flex;
  flex-direction: column;
  gap: 25px;
  align-items: center;
  padding: 70px 0;
}
li {
  position: relative;
  width: 100%;
  color: #4d4d4d;
  max-width: 500px;
  background-color:#f1f1f1;
  box-shadow: 0px 0px 10px rgb(207, 207, 207);
  border-radius: 5px;
  padding: 15px;
  justify-content: center;
  line-height: 1em;
  text-transform: uppercase;
}
li p{
  margin-bottom: 5px;
}
li img{
  margin-top: 6px;
  height: 200px;
  border-radius: 5px;
  box-shadow: 0px 0px 10px rgb(207, 207, 207);
}

</style>
