<template>
  <div id="app">
    <v-style v-if="dark">
      html {
        -webkit-filter: invert(1);
        filter: invert(1);
      }
      img,input,button {
        -webkit-filter: invert(1);
        filter: invert(1);
      }
    </v-style>
    <h1>Image search</h1>
    <h6>By Tigran Arshakyan</h6>
    <button class="btn" @click="dark=!dark" type="button"><span>Dark / Light</span></button>
    <input v-model="query" @keypress.enter="searchImages" type="text" placeholder="Search image by query">
    <div v-if="photos.length && photos[0] != '-L'">
      <div class="images-container" v-for="(photo,i) of photos" :key="i">
        <a target="_blank" :href="photo.src.large2x">
          <img :style="'max-width:100%;border-radius: 1rem;border: 2px solid ' + '#' + (Math.random().toString(16) + '000000').substring(2,8).toUpperCase() + ';'" :src="photo.src.medium">
        </a>
      </div>
    </div>
    <div v-else-if="!photos.length">
      <h1 style="font-family: 'Syne Mono', monospace; margin-top:4rem;font-size:4rem;" >No results</h1>
    </div>
    <div v-else>
      <h1>Try to Search</h1>
    </div>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      query: '',
      dark: false,
      photos: ['-L']
    }
  },
  methods: {
    searchImages : async function() {
      if (this.query.toLowerCase() != 'turkey' && this.query.toLowerCase() != 'azerbaijan') {
        const request = await fetch(`https://api.pexels.com/v1/search?query=${this.query}&per_page=250`,
        {headers: {'Authorization': '563492ad6f91700001000001a7b17b7bcbf24d93a20e7e259123fc01'}})
        const result = await request.json()
        this.photos = await result.photos
      }
    }
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Open+Sans:wght@600&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Syne+Mono&display=swap');

* {
  outline: none;
}

body {
  background: #d4d4d4;
}

#app {
  font-family: 'Open Sans', Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #313131;
  margin-top: 60px;
}

input {
  border: 2px solid #ff5596;
  border-radius: 5px;
  transition-duration: 0.3s;
}

input[placeholder] {
  font-family: 'Open Sans';
  font-size: 1rem;
  color: #ff5596;
}

input::-moz-placeholder {
  font-family: 'Open Sans';
  font-size: 1rem;
  color: #ff5596;
}

input:-moz-placeholder {
  font-family: 'Open Sans';
  font-size: 1rem;
  color: #ff5596;
}

input:-ms-input-placeholder {
  font-family: 'Open Sans';
  font-size: 1rem;
  color: #ff5596;
}

::-webkit-input-placeholder {
  font-family: 'Open Sans';
  font-size: 1rem;
  color: #ff5596;
}

::-moz-placeholder {
  font-family: 'Open Sans';
  font-size: 1rem;
  color: #ff5596;
}

:-moz-placeholder {
  font-family: 'Open Sans';
  font-size: 1rem;
  color: #ff5596;
}

:-ms-input-placeholder {
  font-family: 'Open Sans';
  font-size: 1rem;
  color: #ff5596;
}

input:focus {
  box-shadow:0 0 4px #ff5596, 0 0 10px #ff5596, 0 0 14px #ff5596 ;
}

.images-container {
  display: inline-flex;
  padding: 1rem;
  margin: 1rem;
  justify-content: center;
}


.btn {
  position: relative;

  display: block;
  margin: 30px auto;
  padding: 0;

  overflow: hidden;

  border-width: 0;
  outline: none;
  border-radius: 2px;
  box-shadow: 0 1px 4px rgba(0, 0, 0, .6);
  
  background-color: #2ecc71;
  color: #ecf0f1;
  
  transition: background-color .3s;
}

.btn:hover, .btn:focus {
  background-color: #27ae60;
}

.btn > * {
  position: relative;
}

.btn span {
  display: block;
  padding: 6px 12px;
}

.btn:before {
  content: "";
  
  position: absolute;
  top: 50%;
  left: 50%;
  
  display: block;
  width: 0;
  padding-top: 0;
    
  border-radius: 100%;
  
  background-color: rgba(236, 240, 241, .3);
  
  -webkit-transform: translate(-50%, -50%);
  -moz-transform: translate(-50%, -50%);
  -ms-transform: translate(-50%, -50%);
  -o-transform: translate(-50%, -50%);
  transform: translate(-50%, -50%);
}

.btn:active:before {
  width: 120%;
  padding-top: 120%;
  
  transition: width .2s ease-out, padding-top .2s ease-out;
}

</style>
