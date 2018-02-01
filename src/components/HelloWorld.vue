<template>
  <div>
    <article v-for="(location, idx) in locations" :key="idx">
      <img :src="location.image" width="300px">
      <h1>{{ location.name }}</h1>
      <button class="button is-small is-danger" @click="deleteLocation(location.id)">
        Delete
      </button>
    </article>

    <hr>

    <form @submit="addLocation(name, image)">
      <h2>Add a New Location</h2>
      <input v-model="name" placeholder="Location Name" class="input">
      <input v-model="image" placeholder="Location Image URL" class="input">
      <button type="submit" class="button is-success">Add New Location</button>
    </form>
  </div>
</template>

<script>

import { db } from '../main'

export default {
  name: 'HelloWorld',
  data () {
    return {
      locations: [],
      name: '',
      image: ''
    }
  },
  firestore () {
    return {
      locations: db.collection('locations').orderBy('createdAt')
    }
  },
  methods: {
    addLocation (name, image) {
      const createdAt = new Date()
      db.collection('locations').add({ name, image, createdAt })
      // Clear values
      this.name = ''
      this.image = ''
    },
    deleteLocation (id) {
      db.collection('locations').doc(id).delete()
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
input, button {
  margin-bottom: 10px;
}
</style>
