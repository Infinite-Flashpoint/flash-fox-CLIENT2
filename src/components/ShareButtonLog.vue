<template>
  <div class="share-button-container">
    <a target=_blank :href="sharelinktwitter"  class="twitter"  onclick="javascript:window.open(this.href, '', 'menubar=no,toolbar=no,resizable=yes,scrollbars=yes,height=600,width=600');return false;" >
    <img src="https://img.icons8.com/cute-clipart/30/000000/twitter.png">  
    </a>
    <div class="fb-share-button" :data-href="sharelinkfacebook" data-layout="button_count" data-size="small">
      <a target="_blank" :href="sharelinkfacebook" class="fb-xfbml-parse-ignore" onclick="javascript:window.open(this.href, '', 'menubar=no,toolbar=no,resizable=yes,scrollbars=yes,height=600,width=600');return false;">
      <img src="https://img.icons8.com/dusk/30/000000/facebook-new--v2.png">
      </a>
    </div>

  </div>
</template>

<script>
export default {
  data() {
    return {
    }
  },
  props: {
    sharelinktwitter: String,
    sharelinkfacebook: String
  }
}
</script>

<style scoped>
  .share-button-container {
    display: flex;
    width: 45%;
    margin: auto;
    justify-content: space-between;
    margin: 10px;
  }
  .share-button {
    background: #a15656;
    border: none;
    height: 50px;
    width: 50px;
    border-radius: 50%;
    opacity: 1;
    transition: 0.3s;
  }

  .share-button:hover {
    opacity: 0.7;
    background: #d83333;
  }
  .share-button i{
    color: #ffffff;
    height: 60px;
    width: 30px;
  }
</style>