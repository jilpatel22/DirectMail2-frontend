<script>
import axios from 'axios';

export default {

  data() {
    

    return {
      list: [],
      fields: ["Name", "Date", "File link"],
      loading: false,
    }
  },

  
  created() {
    // watch the params of the route to fetch the data again
    this.$watch(
      () => this.$route.params,
      () => {
        this.fetchData()
      },
      // fetch the data when the view is created and the data is
      // already being observed
      { immediate: true }
    )
  },

  methods: {
    fetchData() {
      this.loading = true
      axios.get('http://localhost:8000/api/mailinglist').then(response =>{
      console.log("Printing the data ",response)
      this.list = response.data
      this.loading = false
    }).catch(error => {
      console.log("Error occured: ",error)
    })
    },
  },
}
</script>

<template>
  <div class="about">
    
    <div>
  <div v-if="loading" class="loading">Loading...</div>

      <table class="table table-bordered table-striped">
      <th  v-for="field in fields" :key='field'> 
        {{field}}
       </th>
       <tbody>
      <tr v-for="item in list" :key='item'>
      <td>{{item['name']}}</td>
      <td>{{item['date']}}</td>
      <td><a href={{item.fileLink}}>{{  item['fileLink']}}</a></td>

    </tr>
  </tbody>
    </table>
    </div>
    
  </div>
  
</template>


<style>
@media (min-width: 1024px) {
  .about {
    min-height: 100vh;
    display: flex;
    align-items: center;
  }
}
</style>
