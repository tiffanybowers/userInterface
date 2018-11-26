<template>
<section>
<div class="uk-margin-bottom">
<nuxt-link to="/" class="uk-button uk-button-default">Back</nuxt-link>
<nuxt-link :to="'/edit/' + person.idt" class="uk-button uk-align-right uk-button-default">Edit</nuxt-link>
</div>

{{ person }}
{{ address }}
{{ telephone }}

<div class="uk-tile uk-tile-primary uk-padding-small">PERSONAL INFORMATION</div>
<table class="uk-table uk-table-hover uk-table-small" >
<tr>
<th><b><u> Identifier </u></b></th>
<th><b><u> First Name </u></b></th>
<th><b><u> Second Name </u></b></th>
</tr>
<tr>
<td>{{ person.idt }}</td>
<td>{{ person.fname }}</td>
<td>{{ person.sname }}</td>
</tr>
<tr>
<th><b><u> Surname </u></b></th>
<th><b><u> Passport  </u></b></th>
<th><b><u> Country Id </u></b></th>
</tr>
<tr>
<td>  {{ person.surname }}</td>
<td>  {{ person.pas }} </td>
<td> {{ person.fk_country_id }}</td>
</tr>

</table>
</div>

<div class="uk-tile uk-tile-primary uk-padding-small">ADDRESS INFORMATION</div>
<table class="uk-table uk-table-hover uk-table-small" >
<tr>
<th><b><u> Street Number </u></b></th>
<th><b><u> Street Name </u></b></th>
<th><b><u> Suburb </u></b></th>
</tr>

<tr>
<td> {{address.streetnumber}} </td>
<td> {{ address.streetname }} </td>
<td> {{ address.suburb }} </td>
</tr>

<tr>
<th><b><u> City </u></b></th>
<th><b><u> Postal code </u></b></th>
<th><b><u> Current Address </u></b></th>
</tr>

<tr>
<td> {{ address.city }} </td>
<td> {{ address.postal_code }} </td>
<td> {{ address.is_current }} </td>
</tr>

<tr>
  <th><b><u>Description of address</u></b></th>
</tr>
<tr>
  <td> {{ address.description }}</td>
</tr>
</table>


<div class="uk-tile uk-tile-primary uk-padding-small">TELEPHONE INFORMATION</div>
<table class="uk-table uk-table-hover uk-table-small">
<tr>
<th><b><u>Contact Number</u></b></th>
<th><b><u>Description of number</u></b></th>
<th><b><u>Current number</u></b></th>
</tr>
<tr>
<td>{{ telephone.num }} </td>
<td>{{ telephone.description}} </td>
<td>{{ telephone.is_current }} </td>
</tr>
</table>

</section>
</template>

<script>
import axios from 'axios'

export default {
  data() {
    return {
      person : '',
      address: '',
      telephone: ''
    }
  },
  async asyncData(context) {
    let personData =  await axios.post('http://localhost:4567/person-database/person/info', JSON.stringify(context.params))
    let addressData = await axios.post('http://localhost:4567/person-database/address/info', JSON.stringify(context.params))
    let telephoneData = await axios.post('http://localhost:4567/person-database/telephone/info', JSON.stringify(context.params))
    return {person: personData.data, address: addressData.data, telephone: telephoneData.data}

  }
}
</script>
