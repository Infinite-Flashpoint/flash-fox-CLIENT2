<template>
  <main role="main" class="inner mb-auto pt-3 upload justify-content-center">
    <div v-show="uploaddisplay">
      <h1 class="cover-heading font-weight-bold">Welcome to FLASHPOINT!</h1>
      <p class="lead">
        Upload and share files for free
        No registration needed, feel free to upload a files and send to anyone.
      </p>
      <p class="lead">
        <a href="#" @click.prevent="createPDF" class="btn btn-lg btn-secondary font-weight-bold">UPLOAD</a>
      </p>
      <!-- <form
        class="dropzone"
        id="my-awesome-dropzone"></form> -->
  
      <label class="custom-upload" for="input-file">
        <input @change="previewFile" id="input-file" type="file" />
      </label>
      <h1 class="p-6">{{url}}<h1>
        <h2 class="p-6">{{censor}}<h2>
      <ShareButton v-show="sharebuttondisplay" :sharelink="url"></ShareButton>
      <button class="btn btn-danger" @click="hideUpload" style="margin: 20px;">See Log Page</button>
    </div>
    <div v-show="logdisplay">
      <FileLog ></FileLog>
      <button class="btn btn-danger" @click="showUpload" style="margin-left: 20px;">See Upload Page</button>
    </div>  
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
      url : "",
      sharebuttondisplay: false,
      uploaddisplay: true,
      logdisplay: false,
      censor: "Safe"
    };
  },
  methods: {
    hideUpload() {
      this.uploaddisplay = false;
      this.logdisplay = true;
    },
    showUpload() {
      this.uploaddisplay = true;
      this.logdisplay = false;
    },
    sendingEvent(file, xhr, formData) {
      formData.append("image", file);
    },
    previewFile(event) {
      this.formCreatePdf.pdf = event.target.files[0];
    },
    createPDF() {
      this.censor="Safe"
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
          this.censor = data.message
          return axios({
              url : "http://flashpoint-server.panjisn.online",
              method : 'POST',
              data : {
                  link : this.url
              }
          })
        })
        .then(_=>{
          this.sharebuttondisplay = true
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