<template>
  <div class="d-flex w-100 h-100 flex-column">
    <Header></Header>
      <ContentUpload></ContentUpload>
      <faq></faq>
      <Footer></Footer>
  </div>
</template>

<script>
import Footer from "./components/Footer";
import Header from "./components/Header";
import ContentUpload from "./components/ContentUpload";
import faq from "./components/faq"
export default {
  components: {
    Footer,
    Header,
    ContentUpload,
    faq
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