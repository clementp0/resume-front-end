<template>
  <transition name="translate" mode="out-in">
  <div v-if="!isloading && !error">
    <div class="intro" v-for="introData in intro" v-bind:key="introData">
      <h1>{{ introData.hey }}</h1>
      <p><b>{{ introData.intro }}</b><br />
        {{ introData.baseline }}
      </p>
      <p>{{ introData.resume }}</p><br />
      <a class="resumeviewer" v-bind:href="'/resume/' + lang">View</a>
      <b> in</b><select v-model="lang">
        <option value="fr">🇫🇷</option>
        <option value="en">🇬🇧</option>
        <option value="pl">🇵🇱</option>
      </select>
    </div>
  </div>
</transition>
<img v-if="isloading || error" alt="loading_ico" src="../images/loading.svg">
</template>

<script>
import axios from 'axios'

export default {
  name: 'HomePage',
  data() {
    return {
      lang: 'en',
      intro: 'null',
      isloading: true,
      error: false,
      log:''
    }
  },

  mounted() {
    axios
      .get(process.env.VUE_APP_API_INTRO)
      .then(response => (this.intro = response.data))
      .catch(error => {
        this.error = true
        this.log = error
      })
      .finally(() => this.isloading = false)
  },
  activated(){
    document.title = process.env.VUE_APP_TITLE + ' | ' + 'Home'
  }
};
</script>

<style scoped>
select{
  margin-left: 5px;
}
select:focus {
    outline: none;
}
.intro {
  text-align: left;
}

select {
  background-color: transparent;
  border: none;
  color: #919191;
}

.resumeviewer {
  background-color: rgb(255, 48, 165);
  text-decoration: none;
  color: white;
  padding: 5px 15px 5px 15px;
  font-weight: 600;
  border-radius: 11px;
}
</style>
