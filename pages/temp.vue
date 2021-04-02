<template>
  <div class="container">
    <div>
      <h1 class="title">Data page</h1>
      <NuxtLink to="/">Homepage</NuxtLink>
      <p v-if="$fetchState.pending">Fetching data...</p>
      <p v-else-if="$fetchState.error">An error occurred :(</p>
      <div v-else>
        <h1>Nuxt Data</h1>
        <ul>
          <li v-for="item of items" :key="item.ID.S">
            {{ item.ID.S }}
          </li>
        </ul>
        <button @click="$fetch">Refresh</button><br />
        <input v-model="inputVal" /><button
          @click="
            addItem()
            $fetch
          "
        >
          Add
        </button>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      items: [],
      inputVal: null,
    }
  },
  async fetch() {
    this.items = await fetch(
      'https://xesfh77f2i.execute-api.us-east-2.amazonaws.com/dev'
    )
      .then((res) => res.json())
      .then((res) => res.body.Items)
  },
  methods: {
    addItem() {
      this.$axios
        .$post('/dev', {
          name: this.inputVal,
        })
        .then(() => this.$fetch())
    },
  },
}
</script>
