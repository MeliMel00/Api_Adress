<template>
  <section>
    <form>

  <p>
    <label for="name">My Adresse</label>
    <input
      id="adresse"
      v-model="checkaddress"
      type="text"
      v-on:click="LoadApi()">
  </p>
 <!-- <div style="">
   <p>{{realaddress}}</p>
 </div> -->
  <p>
    <label for="numero">Number Adress</label>
    <input
      id="numero"
      v-model="numero"
      type="number"
      name="numero"
      min="0"
      disabled="disabled"
    >
  </p>
   <p>
    <label for="name">Adresse</label>
    <input
      id="adresse"
      v-model="adresse"
      type="text"
      name="adresse"
      disabled="disabled"
    >
  </p>
   <p>
    <label for="code Postal">Code Postal</label>
    <input
      id="postal"
      v-model="postal"
      type="number"
      name="postal"
      min="0"
      disabled="disabled"
    >
  </p>
  <p>
    <label for="city">City</label>
    <input
      id="city"
      v-model="city"
      type="text"
      name="city"
      disabled="disabled"
    >
  </p>

</form>
  </section>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
        checkaddress: null,
        adresse: null,
        numero: null,
        postal: null,
        city: null,
        realaddress: [],

    }
  },
  methods:{
    checkForm: function (e) {
      if (this.name && this.age) {
        return true;
      }

      if (!this.adresse) {
        this.errors.push('Need adress');
      }
      e.preventDefault();
    },
    LoadApi: function(){
      if (this.checkaddress) {
        var checkaddress = this.checkaddress
      checkaddress = checkaddress.replace(/ /g,"%")
      var lateadress = checkaddress. normalize('NFD'). replace(/[\u0300-\u036f]/g, "")
      console.log(lateadress)
      axios.get(`https://api-adresse.data.gouv.fr/search/?q=${lateadress}`)
        .then(response => {
          this.realaddress = response.data.features[0]
          this.postal = response.data.features[0].properties.postcode
          this.numero = response.data.features[0].properties.housenumber
          this.adresse = response.data.features[0].properties.street
          this.city = response.data.features[0].properties.city
        })
        .catch(e => {
          this.errors.push(e)
    })
      }
      
    }
  },
  created() {
    
  }
}
</script>

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
form{
  display:flex;
  flex-direction: column;
  align-items: flex-start;
}
</style>
