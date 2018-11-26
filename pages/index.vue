<template>
  <section class="uk-container">
    <article>
      <div class="uk-tile uk-tile-secondary uk-padding-small">
        <p style="color: white;">SEARCH FOR PERSON</p>
      </div>

      <div class="uk-inline uk-margin-small">
          <a @click.prevent="searchP" class="uk-form-icon uk-form-icon-flip"><span uk-icon="icon: search"></span></a>
          <input v-model="search" class="uk-input uk-form-medium uk-form-width-medium" type="search" @keyup.enter="searchP" placeholder="Search" />
      </div>

      <div>
        <!--result label-->
        <span class="uk-label uk-label-warning">Results found :  {{ persons.length }}</span>
      </div>

    <div>
        <!--Add a person-->
        <button style="border-radius: 4px;" class="uk-button uk-button-default uk-float-right"><nuxt-link to="/addPerson" style="color: black;">Add New Person</nuxt-link></button>
    </div>
</article>

<div>
  <div class="uk-container" >
        <table class="uk-table  uk-table-striped uk-table-hover uk-table-small">
          <thead>
            <tr>
              <th>Identifier</th>
              <th>FirstName</th>
              <th>SecondName</th>
              <th>SurName</th>
              <th>Passport</th>

            </tr>
          </thead>
          <tbody>
            <tr v-for="person in persons" >
              <td>{{ person.idt }}</td>
              <td>{{ person.fname }}</td>
              <td>{{ person.sname }}</td>
              <td>{{ person.surname }}</td>
              <td>{{ person.pas }}</td>
              <td><nuxt-link :to="'/view/' + person.idt" class="uk-button uk-button-secondary" style="border-radius: 4px;">View</nuxt-link></td>
              <!-- <td><nuxt-link class="uk-button uk-button-secondary" style="border-radius: 4px;">Delete</nuxt-link></td> -->
            </tr>
          </tbody>
        </table>
        </div>
</div>
 </section>

</template>

<script>
import axios from 'axios'

export default {
  head() {
    return {
      title: 'Search & Add'
    }
  },
  data: function(){
    return{
      persons: [],
      search: ''
    }
  },
  methods:{
    async searchP(){
      let vm = this
        await axios.post('http://localhost:3000/api/person-database/person/search',{
             input : vm.search
          })
        .then(function (response) {
          vm.persons = response.data
          console.log(response.data)
        })
    }
  }
}
</script>
