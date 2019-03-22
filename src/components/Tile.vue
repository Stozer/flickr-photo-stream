<template>
  <div class="tile">
    <div class="photoContainer" v-on:click="photoOpen = true">
      <img class="photo" v-bind:src="item.media.m">
    </div>
    <div class="textContainer">
      <a class="title" v-bind:href="item.link">
        {{ getTitle() }}
      </a>
      <p class="author">
        by <a class="authorLink" v-bind:href="getAuthorLink()">{{ getAuthor() }}</a>
      </p>
      <p v-if="getDescription()"><b>Description:</b> {{ getDescription() }}</p>
      <p v-if="getTags()"><b>Tags:</b> {{ getTags() }}</p>
    </div>
    <Photo v-if="photoOpen" v-on:close="photoOpen = false" v-bind:item="item"></Photo>
  </div>
</template>

<script>
import $ from 'jquery'
import Photo from './Photo'

export default {
  name: 'Tile',
  data () {
    return {
      photoOpen: false
    }
  },
  components: {
    Photo
  },
  props: [
    'item'
  ],
  methods: {
    getTitle: function () {
      const trimmed = $.trim(this.item.title)
      if (!trimmed) {
        return 'Untitled'
      } else if (trimmed.length > trimmed.substr(0, 50).length) {
        return trimmed.substr(0, 50) + '...'
      } else {
        return trimmed
      }
    },
    getAuthor: function () {
      if (!this.item.author) {
        return 'Anon'
      }
      
      const authorArray = this.item.author.split(/\("|"\)/)
      if (authorArray.length === 3) {
        try {
          return unescape(authorArray[1])
        } catch (e) {
          return authorArray[1]
        }
      } else {
        return this.item.author
      }
    },
    getDescription: function () {
      var span = document.createElement('span')
      span.innerHTML = this.item.description
      const contents = span.textContent || span.innerText
      return $.trim(contents.split('posted a photo:')[1])
    },
    getTags: function () {
      const tagsArray = this.item.tags.split(' ')
      return tagsArray.join(', ')
    },
    getAuthorLink: function () {
      return this.item.link.split('/').slice(0, -2).join('/')
    }
  }
}
</script>

<style scoped>
.tile {
  width: 300px;
  height: 380px;
  max-width: 100%;
  flex-grow: 1;
  margin: 20px;
  overflow: scroll;
  box-shadow: 1px 1px 6px #bbb;
  transition: box-shadow 0.2s ease-in-out;
  border-radius: 3px;
  background: silver;
}
.photoContainer {
  width: 100%;
  height: 260px;
  display: flex;  
  align-items: center;
  justify-content: center;
  border-bottom: 1px solid #ddd;
  background: white;
}
.textContainer {
  padding: 10px 10px 0 10px;
  text-align: left;
  font-size: 13px;
  height: 109px;
  overflow: scroll;
}
.photo {
  max-width: 90%;
  max-height: 200px;
  transition: transform 0.2s ease-in-out;
}
.photoContainer:hover .photo {
  transform: scale(1.2);
  opacity: 0.5;
}
.title {
  font-weight: 900;
  font-size: 20px;
  color: #333;
  text-decoration: none;
  margin-bottom: 0;
  margin-top: 0;
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
  display: block;
}
.title:hover {
  text-decoration: none;
}
.author {
  margin-top: 5px;
  font-size: 15px;
}
.authorLink {
  font-weight: 900;
  color: #333;
  text-decoration: none;
}
.authorLink:hover {
  text-decoration: none;
}
</style>