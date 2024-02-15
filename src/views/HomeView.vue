<template>
  <v-container>
  <v-row>
  <v-col cols="12">
   <v-text-field
        v-model="search"
        append-icon="mdi-magnify"
        label="Search by title"
        single-line
        hide-details
        :loading="isLoading"
        
      ></v-text-field>
  </v-col>
  <v-col cols="6">
   <v-text-field
        v-model="author"
        append-icon="mdi-magnify"
        label="Search by author"
        single-line
        hide-details
        :loading="isLoadingAuthor"
        
      ></v-text-field>
  </v-col>
  </v-row>
        <v-row>
          <v-col
            v-for="book in books"
            :key="book.title"
            cols="4"
            md="3"
          >
       <v-card height="200">
            <h3>{{book.title}}</h3>
            <h5>{{book.author}}</h5>
            </v-card>

          </v-col>
        </v-row>
        <v-col cols="12">
          <v-pagination
      v-model="page"
      :length="Math.ceil(totalBooks/perPage)"
      :total-visible="7"
    ></v-pagination>
        </v-col>
        <v-row> 
   
        </v-row>
      </v-container>
</template>

<script>
  
  export default {
    name: 'HomeView' ,

    
 data() {
  return {
    test: 'Testna varijabla',
    books : [],
    page: 1,
    totalBooks: 0,
    perPage: 20,
    search: '',
    author: '',
    isLoading: false,
    isLoadingAuthor: false
  }
  },



 created(){
 console.log('created')
  this.getData();
 },

 methods: {
  getData() {
    let api ="https://api.nytimes.com/svc/books/v3/lists/best-sellers/history.json"
    
    this.axios.get(api, {
      params: {
        'api-key': 'MOmOdyMp2WeAFbW5BHGWlCzoTPHXIZLA',
        'offset': this.perPage * (this.page -1),
        'title': this.search,
        'author':this.author
      }
    }).then((response) => {
  console.log(response.data)
  this.books = response.data.results
  this.totalBooks = response.data.num_results
  this.isLoading = false
   })
   },



  
     fetchEntriesDebounced() {
      clearTimeout(this._searchTimerId)
      this._searchTimerId = setTimeout(() => {
        this.getData()
      }, 700) /* 500ms throttle */
    
   },
   searchEntries() {
      this.books = []
      this.page = 1
      this.fetchEntriesDebounced()
   }
  },

 watch:{
   page:function() {  
    this.getData();
    },
    search (val) {
      if (!val) {
        return
      }
      this.isLoading = true
      this.searchEntries()
     },

    author (val) {
      if (!val) {
        return
      }
      this.isLoadingAuthor = true
      this.searchEntries();
    }
  }
}
  

</script>

