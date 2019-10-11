<template>
  <main role="main" class="inner mb-auto pt-3 upload justify-content-center">
    <h1 class="cover-heading font-weight-bold">Welcome to FLASHPOINT!</h1>

    <p class="lead">
      Upload and share files for free
      No registration needed, feel free to upload a files and send to anyone.
    </p>
    <p class="lead">
      <a href="#" @click.prevent="createPDF" class="btn btn-lg btn-secondary font-weight-bold">UPLOAD</a>
    </p>
      <input @change="previewFile" id="input-file" type="file" accept="application/pdf" />
    <!-- <form
      class="dropzone"
      id="my-awesome-dropzone"></form> -->
    <h1>{{url}}<h1>
  </main>
</template>

<script>
import vue2Dropzone from "vue2-dropzone";
import "vue2-dropzone/dist/vue2Dropzone.min.css";
import axios from 'axios'

export default {
  components: {
    vueDropzone: vue2Dropzone
  },
  data: function() {
    return {
      dropzoneOptions: {
        url: "http://flashpoint-server.panjisn.online/upload",
        thumbnailWidth: 150,
        maxFilesize: 0.5,
        headers: { "My-Awesome-Header": "header value" }
      },
      formCreatePdf: {
        pdf: ""
      },
      url : ""
    };
  },
  methods: {
    sendingEvent(file, xhr, formData) {
      formData.append("image", file);
    },
    previewFile(event) {
      this.formCreatePdf.pdf = event.target.files[0];
    },
    createPDF() {
      if (!this.formCreatePdf.pdf){
        this.$swal.fire({
            type: "error",
            title: "failed to upload file ",
            text: 'Cannot be empty',
            showConfirmButton: false,
            timer: 2000
          });
      } else {
        this.$swal.fire({
        title: "wait a minute to upload data",
        allowOutsideClick: () => !this.$swal.isLoading()
      });
      this.$swal.showLoading("wait a minute ");

        let { title, description, pdf } = this.formCreatePdf;
      var bodyFormData = new FormData();
      bodyFormData.append("image", pdf);

      axios({
        url: "http://flashpoint-server.panjisn.online/upload",
        method: "POST",
        data: bodyFormData
      })
        .then(({data}) => {
          this.$swal.close();
          this.$swal.fire({
            type: "success",
            title: "successfully upload file",
            showConfirmButton: false,
            timer: 2000
          });
          console.log(data)
          this.url = data.link
          return axios({
              url : "http://flashpoint-server.panjisn.online",
              method : 'POST',
              data : {
                  link : this.url
              }
          })
        })
        .then(_=>{
        })
        .catch(err => {
            console.log(err)
          let message =
            (err.response && err.response.data && err.response.data.message) ||
            "error failed to upload file";
          this.$swal.fire({
            type: "error",
            title: "failed to upload file ",
            text: message,
            showConfirmButton: false,
            timer: 2000
          });
        });
      }
    }
  }
};
</script>

<style>
</style>


