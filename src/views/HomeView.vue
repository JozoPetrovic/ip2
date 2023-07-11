<template>
  <v-container>
        <v-row>
          <v-col
            v-for="n in 24"
            :key="n"
            cols="4"
          >
            <v-card height="200">
            {{test}}
            </v-card>
          </v-col>
        </v-row>
      </v-container>
</template>

<script>
  

  export default {
    name: 'HomeView',

data() {
  return {
    test: 'dsdsd',
    Movie : []
  }
},



created(){
console.log('created')
this.getData();
},


methods: {
  getData() {
    let api ="https://api.nytimes.com/svc/movies/v2/critics/{reviewer}.json"
    
    this.axios.get(api, {
      params: {
        'api-key': 'MOmOdyMp2WeAFbW5BHGWlCzoTPHXIZLA'
      }
    }).then((response) => {
  console.log(response.data)
  this.Movie = response.data.resoults
})
  }
}


  }
</script>
