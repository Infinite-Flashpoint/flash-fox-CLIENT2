<template>
  <main role="main" class="inner mb-auto pt-3 upload justify-content-center">
    <h1 class="cover-heading font-weight-bold">Welcome to FLASHPOINT!</h1>

    <p class="lead">
      Upload and share files for free
      No registration needed, feel free to upload a files and send to anyone.
    </p>
    <p class="lead">
      <a href="#" class="btn btn-lg btn-secondary font-weight-bold">UPLOAD</a>
    </p>
    <vue-dropzone
      ref="myVueDropzone"
      id="dropzone"
      :options="dropzoneOptions"
      v-on:vdropzone-sending="sendingEvent"
    ></vue-dropzone>
    <label class="custom-upload" for="input-file">
      <input @change="previewFile" id="input-file" type="file" />
    </label>
    <button @click.prevent="createPDF" variant="outline-primary">Submit PDF</button>
    <h1>{{url}}<h1>
    <ShareButton :sharelink="url"></ShareButton>
    <FileLog></FileLog>
  </main>
</template>

<script>
import vue2Dropzone from "vue2-dropzone";
import "vue2-dropzone/dist/vue2Dropzone.min.css";
import axios from 'axios';
import ShareButton from "./ShareButton";
import FileLog from "./FileLog";

export default {
  components: {
    ShareButton,
    FileLog,
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
      console.log(file);
      formData.append("image", file);
    },
    previewFile(event) {
      this.formCreatePdf.pdf = event.target.files[0];
    },
    createPDF() {
      // Swal.showLoading();
      /* this.$swal.fire({
        title: "wait a minute to upload data",
        allowOutsideClick: () => !this.$swal.isLoading()
      });
      this.$swal.showLoading("wait a minute "); */

      let { title, description, pdf } = this.formCreatePdf;
      var bodyFormData = new FormData();
      bodyFormData.append("image", pdf);

      let token = localStorage.getItem("token");

      axios({
        url: "http://flashpoint-server.panjisn.online/upload",
        method: "POST",
        data: bodyFormData
      })
        .then(({data}) => {
          // console.log(response.data);
          //this.$emit("triggerReload");

          // FB.XFBML.parse();
          /* this.$swal.close();
          this.$swal.fire({
            type: "success",
            title: "successfully upload data",
            showConfirmButton: false,
            timer: 2000
          }); */
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
        .then(({data})=>{
            console.log(data)
        })
        .catch(err => {
            console.log(err)
          /* let message =
            (err.response && err.response.data && err.response.data.message) ||
            "error failed to upload data"; */
          // console.log("masuk ke error")
          // this.$swal('Hello Vue world!!!')
          /* this.$swal.fire({
            type: "error",
            title: "failed to upload data ",
            text: message,
            showConfirmButton: false,
            timer: 2000
          }); */
          // console.log(err.response)
        });
    }
  }
};
</script>

<style>
</style>


