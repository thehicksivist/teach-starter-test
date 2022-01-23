<template>
  <div class="container">
    <h1>{{ msg }}</h1>
    <select>
      <option default value="">
        ---Select a city to view the current weather---
      </option>
      <option value="brisbane" >
        Brisbane
      </option>
      <option value="sydney">
        Sydney
      </option>
      <option value="melbourne">
        Melbourne
      </option>
    </select>
    <h3>City Name</h3>
    <p>Icon and Temperature</p>
  </div>
</template>
<script>
export default {
  name: 'WeatherWidget',
  props: {
    msg: String
  },
  data() {
    return {
      posts: [],
    };
  },

  methods: {
    async getData() {
      try {
        let response = await fetch("http://jsonplaceholder.typicode.com/posts");
        this.posts = await response.json();
      } catch (error) {
        console.log(error);
      }
    },
  },

  created() {
    this.getData();
  },
}
</script>

<script>
export default {
  data() {
    return {
      posts: [],
    };
  },

  methods: {
    async getData() {
      try {
        let response = await fetch("http://jsonplaceholder.typicode.com/posts");
        this.posts = await response.json();;
      } catch (error) {
        console.log(error);
      }
    },
  },

  created() {
    this.getData();
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
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
</style>
