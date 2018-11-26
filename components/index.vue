<template>

  <div v-component= "my-component"></div>

</template>

<script>

Vue.component('api/person-database/person/search', {
  data: function() {
    return {
      persons: [],
      search: ''
    }
  },
  methods: {
    async searchP(){
      let vm = this
        await axios.post('http://localhost:3000/api/person-database/person/search',{
             input :this.search
          })
        .then(function (response) {
          console.log( response.data)
        })
    }
  }
})
</script>
