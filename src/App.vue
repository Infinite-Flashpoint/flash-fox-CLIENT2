<template>
  <div class="d-flex w-100 h-100 flex-column">
    <Header></Header>
      <ContentUpload></ContentUpload>
      <Footer></Footer>
  </div>
</template>

<script>
import Footer from "./components/Footer";
import Header from "./components/Header";
import ContentUpload from "./components/ContentUpload"
export default {
  components: {
    Footer,
    Header,
    ContentUpload
  },
  data() {
    return {
      message: "Hello world"
    };
  }
};
</script>

<style scoped>
</style>