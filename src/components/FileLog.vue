<template>
  <div class="filelog-container">
    <button class="btn btn-danger" @click="showLogs">See Log</button>
    <ul class="list-group">
      <li v-for="link in links" :key="link.id" class="list-group-item">
        {{ link.link }}
        <ShareButtonLog
        :sharelinktwitter="`https://twitter.com/intent/tweet?text=${link.link}`"
        :sharelinkfacebook="`https://www.facebook.com/sharer/sharer.php?u=${link.link}`"></ShareButtonLog>
      </li>
    </ul>
  </div>
</template>
<script>
import Axios from 'axios'
import ShareButtonLog from './ShareButtonLog'
export default {
  components: {
    ShareButtonLog
  },
  data() {
    return {
      links: []
    }
  },
  methods: {
    showLogs() {
      console.log('masuk see log')
      Axios.get('http://flashpoint-server.panjisn.online')
      .then(({data}) => {
        console.log(data)
        data.forEach(element=>{
          this.links.unshift(element)
        })
        // this.links = data
      })
    }
  }
}
</script>
<style>
.list-group {
  font-size: 15px;
  color: rgb(141, 60, 60);
  width: 50%;
  margin: auto;
}
div ul{height:400px; width:18%;}
div ul{overflow:hidden; overflow-y:scroll;}
</style>