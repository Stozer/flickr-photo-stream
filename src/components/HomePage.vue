<template>
  <div class="container">
    <div class="navMenu">
      <img src="../assets/logo.png" class="logo"/>
      <h2>Photo Stream</h2>
      <div class="searchMenu">
        <input
          class="searchInput"
          type="text"
          placeholder="Search..."
          v-model="tags"
          v-on:keyup.enter="search">
        <button
          class="searchButton"
          v-on:click="search">Search</button>
      </div>
    </div>
    <div class="grid"> 
      <Tile
        v-for="(item, index) in items"
        v-bind:item="item"
        v-bind:key="index">
      </Tile>
    </div>
  </div>
</template>

<script>
import $ from 'jquery'
import Tile from './Tile'

export default {
  name: 'HomePage',
  components: {
    Tile
  },
  data: () => ({
    items: [],
    tags: ''
  }),
  mounted () {
    $.ajax({
      url: 'https://api.flickr.com/services/feeds/photos_public.gne?format=json&jsoncallback=?',
      dataType: 'json'
    }).then((data) => {
      this.items = data.items
      // eslint-disable-next-line
      console.log("Items : " + JSON.stringify(this.items))
    })
  },
  methods: {
    search: function () {
      const tags = this.tags.split(/ |,/).map($.trim).filter((el) => !!el.length).join(',')
      $.ajax({
        url: `https://api.flickr.com/services/feeds/photos_public.gne?tags=${tags}&format=json&jsoncallback=?`,
        dataType: 'json'
      }).then((data) => {
        this.items = data.items
        // eslint-disable-next-line
        console.log("Tags : " + JSON.stringify(this.tags))
        // eslint-disable-next-line
        console.log("Items from search : " + JSON.stringify(this.items))
      })
    }
  }
}
</script>

<style scoped>
.container {
  text-align: left;
}
.navMenu {
  padding: 10px;
  display: flex;
  justify-content: space-between;
  background-color: silver;
}
.logo {
  width: 5rem;
  height: 2rem;
  display: flex;
  padding: 8px;
}
h2 {
  margin: 0px;
  padding: 8px;
}
.searchMenu {
  display: flex;
  min-height: 43px;
  padding: 3px 0;
  width: 40%;
}
.searchInput {
  box-sizing: border-box;
  font-size: 20px;
  display: block;
  padding: 3px 8px;
  margin-right: 10px;
  border-radius: 3px;
  border: 1px solid #ccc;
  width: 85%;
}
.searchButton {
  font-size: 20px;
  display: block;
  padding: 3px 8px;
  border-radius: 3px;
  border: 1px solid #888;
  background-color: #f1f1f1;
  color: #333;
}
.grid {
  display: flex;
  flex-wrap: wrap;
}
@media (max-width: 728px) {
  .navMenu {
    flex-direction: column;
  }
  .logo {
    margin-bottom: 10px;
  }
}
</style>