<template>
<section>
<nuxt-link :to="'/view/' + person.idt" class="uk-button uk-button-default uk-margin-bottom">Back</nuxt-link>
<div>

</div>
<div class="uk-tile uk-tile-secondary uk-padding-small">PERSONAL INFORMATION</div>
<table class="uk-table uk-table-hover uk-table-small">
<tr>
    <th><b><u> Identifier </u></b></th>
    <th><b><u> First Name </u></b></th>
    <th><b><u> Second Name </u></b></th>
</tr>
<tr>
<td><input v-model="person.idt" class="uk-input uk-form-small"/></td>
<td><input v-model="person.fname" class="uk-input uk-form-small"/></td>
<td><input v-model="person.sname" class="uk-input uk-form-small"/></td>
</tr>
<tr>
    <th><b><u> Surname </u></b></th>
    <th><b><u> Passport  </u></b></th>
    <th><b><u> Country Id </u></b></th>
</tr>
<tr>
<td><input v-model="person.surname" class="uk-input uk-form-small"/></td>
<td><input v-model="person.pas" class="uk-input uk-form-small"/></td>
<td><input v-model="person.fk_country_id" class="uk-input uk-form-small" /></td>
</tr>

</table>

<div class="uk-tile uk-tile-secondary uk-padding-small">ADDRESS INFORMATION</div>
<table class="uk-table uk-table-hover uk-table-small">
<tr>
    <th><b><u> Street Number </u></b></th>
    <th><b><u> Street Name </u></b></th>
    <th><b><u> Suburb </u></b></th>
    <th><b><u> City </u></b></th>
</tr>

<tr>
<td><input class="uk-input uk-form-small" v-model="address.streetnumber"/></td>
<td><input class="uk-input uk-form-small" v-model="address.streetname"/> </td>
<td><input class="uk-input uk-form-small" v-model="address.suburb"/></td>
<td><input class="uk-input uk-form-small" v-model="address.city" /></td>
</tr>

<tr>
    <th><b><u> Postal code </u></b></th>
    <th><b><u> Current Address </u></b></th>
</tr>

<tr>

<td><input class="uk-input uk-form-small" v-model="address.postal_code"/></td>
<td><input  class="uk-input uk-form-small" v-model="address.is_current"/></td>


</tr>
</table>

<div class="uk-tile uk-tile-primary uk-padding-small">TELEPHONE INFORMATION</div>
<table class="uk-table uk-table-hover uk-table-small">
<tr>
    <th><b><u>Contact Number: </u></b></th>
    <th><b><u>Description of number</u></b></th>
    <th><b><u>Current number</u></b></th>
</tr>
<tr>
<td><input class="uk-input uk-form-small" v-model="telephone.num"/></td>
</tr>
<tr>
<td><input v-model="telephone.num" class="uk-input uk-form-small></td" /></td>
<td><input v-model="telephone.description" class="uk-input uk-form-small" /></td>
<td><input v-model="telephone.is_current" class="uk-input uk-form-small" /></td>
</tr>
</table>

<button class="uk-button uk-button-primary uk-margin-bottom" @click.prevent="loadData">Load</button>

</section>
</template>


<script>
import axios from 'axios'

export default {
  data() {
    return {
      person : {
        idt: '',
        fname: '',
        sname: '',
        tname: '',
        surname: '',
        pas: '',
        countryId: ''
      },
      address: {
        streetnumber: '',
        streetname: '',
        suburb: '',
        city: '',
        postal_code: '',
        is_current: '',
        description: ''
      },
      telephone:{
        num: '',
        is_current: '',
        description: ''
      }
    }
  },
  async asyncData(context) {
    let personData =  await axios.post('http://localhost:4567/person-database/person/info', JSON.stringify(context.params))
    let addressData = await axios.post('http://localhost:4567/person-database/address/info', JSON.stringify(context.params))
    let telephoneData = await axios.post('http://localhost:4567/person-database/telephone/info', JSON.stringify(context.params))
    return { person: personData.data, address: addressData.data, telephone: telephoneData.data }

  },
  methods: {
    loadData() {
      this.personMethod()
      this.telephoneMethod()
      this.addressMethod()
      alert('Successfully updated!!');
    },
    //person update
  async personMethod() {
    await axios({
        method: 'PUT',
        url:'/api/person-database/person/update',
        data: JSON.stringify(this.person)
      })
      .then(function (response) {
        console.log(response.data);
      });
    },
    //telephone update
    async telephoneMethod() {
     await axios({
       method: 'PUT',
       url: '/api/person-database/telephone/update',
       data: JSON.stringify(this.telephone)
     })
    .then(function (response) {
        console.log(response.data);
        });
      },
    //address update
    async addressMethod() {
      await axios({
        method: 'PUT',
        url: '/api/person-database/address/update',
        data: JSON.stringify(this.address)
      })
      .then(function (response) {
        console.log(response.data);
      });
    }
  }
}
</script>
