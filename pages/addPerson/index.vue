<template>
<section>
<div class="uk-container">
  <div class="uk-label uk-label-warning uk-margin-bottom">Add Information</div>

  <div><button class="uk-button uk-button-secondary" style="border-radius: 4px;"><nuxt-link to="/" style="color: white">Back</nuxt-link></button></div>

  <!--person info-->
  <div class="uk-margin-top">
    <form ref="personForm" v-show="showForms.person" >
      <legend class="uk-legend">PERSONAL INFORMATION</legend>

        <table class="uk-table">
          <thead>
            <tr>
                <th class="uk-width-medium"><b>Identifier</b></th>
                <th class="uk-width-medium"><b>Firstname</b></th>
                <th class="uk-width-medium"><b>Secondname</b></th>
            </tr>
          </thead>
          <tbody>
            <tr>
              <td><input class="uk-input uk-form-small" v-model="person.idt" name="idt" /></td>
              <td><input class="uk-input uk-form-small" v-model="person.fname" name="fname" /> </td>
              <td><input class="uk-input uk-form-small" v-model="person.sname" /></td>
            </tr>
          </tbody>

          <tr>
              <th><b>Surname</b></th>
              <th><b>Passport</b></th>
              <th><b>Country Id</b></th>
          </tr>

          <tr>
            <td><input class="uk-input uk-form-small" v-model="person.surname" name="surname" /></td>
              <td><select class="uk-margin uk-select uk-form-small" v-model="person.pas" name="pas" >
                  <option value="true">True</option>
                  <option value="false">False</option>
                </select>
              </td>
              <td>
                  <div class="uk-margin">
                    <select v-model="person.countryId" name="countryId" class="uk-select uk-form-small">
                      <option v-for="country in countrylist" :key="country.countryId">
                        {{country.name}}
                      </option>
                    </select>
                  </div>
              </td>
          </tr>

        </table>

    </form>

    <div id="btnSubmit">
      <button v-on:click.prevent="validateForm()" v-if="showForms.person" type="button" class="uk-button uk-button-secondary uk-float-right" style="border-radius: 4px;">Next</button>
    </div>
  </div>

  <!--Address info-->
  <div class="uk-margin-top">
    <form ref="addressForm" v-show="showForms.address">
      <legend class="uk-legend">ADDRESS INFORMATION</legend>
        <table class="uk-table uk-table-hover uk-table-small">
          <thead>
          <tr>
              <th class="uk-width-medium"><b>Street Number</b></th>
              <th class="uk-width-medium"><b>Street Name</b></th>
              <th class="uk-width-medium"><b>Suburb</b></th>
              <th class="uk-width-medium"><b>City</b></th>
          </tr>
          </thead>
          <tbody>
          <tr>
              <td><input class="uk-input uk-form-small" v-model="address.streetnumber" name="streetnumber"/></td>
              <td><input class="uk-input uk-form-small" v-model="address.streetname" name="streetname"/> </td>
              <td><input class="uk-input uk-form-small" v-model="address.suburb" name="suburb"/></td>
              <td><input class="uk-input uk-form-small" v-model="address.city" name="city"/></td>
          </tr>
          </tbody>
          <tr>
              <th><b>Postal code</b></th>
              <th><b>Description of address</b></th>
              <th><b>Current Address</b></th>
          </tr>

          <tr>
            <td><input class="uk-input uk-form-small" v-model="address.postal_code" name="postal_code" /></td>
            <td><select class="uk-margin uk-select uk-form-small" v-model="address.description" name="description">
                <option>Home Address</option>
                <option>Postal Address</option>
              </select></td>
            <td><select class="uk-margin uk-select uk-form-small" v-model="address.is_current" >
                <option>True</option>
                <option>False</option>
              </select></td>
          </tr>

        </table>
    </form>

    <div id="btnSubmit">
      <button v-on:click.prevent="validateForm2()" v-if="showForms.address" type="button" class="uk-button uk-button-secondary uk-float-right" style="border-radius: 4px;">Next</button>
      <button v-on:click.prevent=" showForms.address = false ; showForms.person = true" v-if="showForms.address" type="button" class="uk-button uk-button-secondary uk-float-right" style="border-radius: 4px; margin-right: 1em;">Previous</button>
    </div>
  </div>

  <div class="uk-margin-top">
    <form ref="telForm" v-show="showForms.telephone">
      <legend class="uk-legend">ADDRESS INFORMATION</legend>
        <table class="uk-table uk-table-small">
          <thead>
            <tr>
              <th class="uk-width-medium"><b>Number</b></th>
              <th class="uk-width-medium"><b>Description of number</b></th>
              <th class="uk-width-medium"><b>Current Number</b></th>
            </tr>
          </thead>

          <tr>
            <td><input v-model="telephone.num" class="uk-input uk-form-small" name="num"/>
            <td><select class="uk-margin uk-select uk-form-small" v-model="telephone.description" name="description">
              <option>Cell</option>
              <option>Home</option>
              </select></td>
            <td><select class="uk-margin uk-select uk-form-small" v-model="telephone.is_current">
              <option value="true">True</option>
              <option value="false">False</option>
            </select></td>
          </tr>
        </table>

      <div class="uk-button-group uk-float-right">
        <button v-on:click.prevent="showForms.address = true; showForms.telephone = false;" v-if="showForms.telephone" type="button" class="uk-button uk-button-secondary" style="border-radius: 4px; margin-right: 1em;">Previous</button>
        <button v-on:click.prevent="validateForm3(); submitInfo();" type="button" class="uk-button uk-button-secondary" style="border-radius: 4px;">Save</button>
      </div>
    </form>
  </div>

</div>
</section>
</template>

<script>

import axios from 'axios';

export default {
  data: function(){
    return{
      showForms: {
        person: true,
        address: false,
        telephone: false
      },
      isAddingPerson: true,
      person: {
        idt: '',
        fname: '',
        sname: '',
        tname: '',
        surname: '',
        pas: false,
        countryId: ''
      },
      address: {
        streetnumber: '',
        streetname: '',
        suburb: '',
        city: '',
        postal_code: '',
        is_current: false,
        description: ''
      },
      telephone: {
        num: '',
        is_current: false,
        description: ''
      },
      countrylist: []
    }
  },
  async asyncData() {
    let countryData = await axios.get('/api/person-database/country/list')
    return { countrylist : countryData.data }
  },
  methods:{
    submitInfo(){
      if(this.validateForm3()){
         this.addPerson()
         this.addPersonAddress()
         this.addPersonTel()
         alert("Successfully added!!")
      }
      else {
        this.showForms.telephone = true;
      }
    },
      validateForm() {
        //person
        this.$refs.personForm["idt"].className = "uk-input";
        this.$refs.personForm["idt"].placeholder = ""
        this.$refs.personForm["fname"].className = "uk-input";
        this.$refs.personForm["fname"].placeholder = ""
        this.$refs.personForm["surname"].className = "uk-input";
        this.$refs.personForm["surname"].placeholder = ""
        this.$refs.personForm["pas"].className = "uk-select";
        this.$refs.personForm["pas"].placeholder = ""
        this.$refs.personForm["countryId"].className = "uk-input";
        this.$refs.personForm["countryId"].placeholder = ""

        let p = this.person;
        let formTest = true

        for(let key in p){
          p[key] = String(p[key]).trim() === "" || p[key] === null ? null : String(p[key]).trim();
        }

        if(p["idt"] === null){
          this.$refs.personForm["idt"].className = "uk-input uk-form-danger";
          this.$refs.personForm["idt"].placeholder = "Required field"
          formTest = false
        }
        if(p["fname"] === null){
          this.$refs.personForm["fname"].className = "uk-input uk-form-danger";
          this.$refs.personForm["fname"].placeholder = "Required field"
          formTest = false
        }
        if(p["surname"] === null){
          this.$refs.personForm["surname"].className = "uk-input uk-form-danger";
          this.$refs.personForm["surname"].placeholder = "Required field"
          formTest = false
        }
        if(p["pas"] === null){
          this.$refs.personForm["pas"].className = "uk-select uk-form-danger";
          this.$refs.personForm["pas"].placeholder = "Required field"
          formTest = false
        }
        if(p["fk_country_id"] === null){
          this.$refs.personForm["countryId"].className = "uk-input uk-form-danger";
          this.$refs.personForm["countryId"].placeholder = "Required field"
          formTest2 = false
        }
        console.log(this.$refs)

        if(formTest){
         this.showForms.person = false;
         this.showForms.address = true
       }
        return formTest;
    },
      validateForm2() {
              //address
              this.$refs.addressForm["streetnumber"].className = "uk-input";
              this.$refs.addressForm["streetnumber"].placeholder = ""
              this.$refs.addressForm["streetname"].className = "uk-input";
              this.$refs.addressForm["streetname"].placeholder = ""
              this.$refs.addressForm["suburb"].className = "uk-input";
              this.$refs.addressForm["suburb"].placeholder = ""
              this.$refs.addressForm["city"].className = "uk-input";
              this.$refs.addressForm["city"].placeholder = ""
              this.$refs.addressForm["description"].className = "uk-margin uk-select";
              this.$refs.addressForm["description"].placeholder = ""


              let a = this.address;
              let formTest2 = true;

              for(let key in a){
                a[key] = String(a[key]).trim() === "" || a[key] === null ? null : String(a[key]).trim();
              }

              if(a["streetnumber"] === null){
                this.$refs.addressForm["streetnumber"].className = "uk-input uk-form-danger";
                this.$refs.addressForm["streetnumber"].placeholder = "Required field"
                formTest2 = false
              }
              if(a["streetname"] === null){
                this.$refs.addressForm["streetname"].className = "uk-input uk-form-danger";
                this.$refs.addressForm["streetname"].placeholder = "Required field"
                formTest2 = false
              }
              if(a["suburb"] === null){
                this.$refs.addressForm["suburb"].className = "uk-input uk-form-danger";
                this.$refs.addressForm["suburb"].placeholder = "Required field"
                formTest2 = false
              }
              if(a["city"] === null){
                this.$refs.addressForm["city"].className = "uk-input uk-form-danger";
                this.$refs.addressForm["city"].placeholder = "Required field"
                formTest2 = false
              }
              if(a["description"] === null){
                this.$refs.addressForm["description"].className = "uk-margin uk-select uk-form-danger";
                this.$refs.addressForm["description"].placeholder = "Required field"
                formTest2 = false
              }
              console.log(this.$refs)

              if(formTest2){
                this.showForms.address = false;
                this.showForms.telephone = true
              }
              return formTest2;
      },
      validateForm3() {

             //telephone
              this.$refs.telForm["num"].className = "uk-input";
              this.$refs.telForm["num"].placeholder = ""
              this.$refs.telForm["description"].className = "uk-input";
              this.$refs.telForm["description"].placeholder = ""

              let t = this.telephone;
              let formTest3 = true

              for(let key in t){
                t[key] = String(t[key]).trim() === "" || t[key] === null ? null : String(t[key]).trim();
              }

              if(t["num"] === null){
                this.$refs.telForm["num"].className = "uk-input uk-form-danger";
                this.$refs.telForm["num"].placeholder = "Required field"
                this.$refs.telForm["description"].className = "uk-input uk-form-danger";
                this.$refs.telForm["description"].placeholder = "Required field"
                formTest3 = false
              }
              console.log(this.$refs)
              return formTest3;
            },
//==================================================================================================================
      async addPerson(){
        await axios.post('api/person-database/person/add', JSON.stringify(this.person))
         .then(function (response) {
           console.log(response.data)
         });
       },
      async addPersonAddress(){
        this.address.idt = this.person.idt
        await axios.post('api/person-database/address/add-person-address', JSON.stringify(this.address))
        .then(function (response) {
          console.log(response.data);
        });
      },
      async addPersonTel(){
        this.telephone.idt = this.person.idt
        await axios.post('api/person-database/telephone/add-person-tel', JSON.stringify(this.telephone))
        .then(function (response) {
          console.log(response.data);
      });
    },
      //next and previous form
      update() {
        setTimeout(() => {
          this.isAddingPerson = false
        }, 900)
        console.log("Updated!!");
      },
      previous() {
        setTimeout(() => {
          this.isAddingPerson = true
        }, 900)
        console.log('It worked!');
      }
  }
}//export end tags
</script>
