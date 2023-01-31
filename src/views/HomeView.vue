<script>
import axios from 'axios';
import AWS from 'aws-sdk';
import fs from 'fs'
export default {
  data() {
    return {
      name: "",
      date: "",
      file: "",
      errorMessage : "",
      error: true,
     showSuccess:false
    };
  },

 

  methods: {
    onSubmit: function(e) {
      
      e.preventDefault();
      axios.post('http://localhost:8000/api/insertMailList',{date: this.date, fileLink:this.url, name: this.name}, {
        headers :{
          "Content-Type" : 'application/json'
        }
      }).then((response)=>{
        this.showSuccess = true
        console.log("Inserted successfully",response)
        e.target.reset();
      }).catch((error)=>{
        console.log("ERROR: ", error)
      });

    },

    onFileUpload: function(e) {
      this.file = this.$refs.file.files[0]      
    },

   

    openUploadModal:function() {
      window.cloudinary.openUploadWidget(
        { cloud_name: import.meta.env.VITE_CLOUDINARY_CLOUD_NAME,
          upload_preset: import.meta.env.VITE_CLOUDINARY_PRESET_NAME
        },
        (error, result) => {
          if (!error && result && result.event === "success") {
            console.log('Done uploading..: ', result.info);  
            this.url = result.info.url;

          }
        }).open();
    }
  },
};
</script>

<template>
  <main>
    <div class="columns" id="app">
    <section class="section">
      <h1 class="title">Mass Mailing</h1>
      <hr>
      <!-- form starts here -->
      <section class="form">
        <form @submit.prevent="onSubmit">
          <div class="field">
            <label class="label">Name</label>
            <div class="control">
              <input name="name" v-model="name" class="input" type="text" placeholder="Full name">
            </div>
            <div class="filed">
            <label class="label" >Date</label>
              <div class="control">
                <input v-model="date" type="date" class="input">
              </div> 
            </div>
              
          </div>
            <div>
              <button @click="openUploadModal">Upload files</button>
            </div>
              
          <div class="field is-grouped">
            <div class="control">
              <button class="button is-primary">
								Submit
							</button>
            </div>
          </div>
        </form>
      </section>
    </section>
   </div>
  </main>
</template>
