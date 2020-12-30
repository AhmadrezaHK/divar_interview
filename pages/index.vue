<template>
  <div class="container">
    <cats v-if="cats" :cats="cats" />
  </div>
</template>

<script>
import Cats from "../components/Cats";
import axios from "axios";
export default {
  components: {
    Cats
  },
  data: () => ({
    cats: null
  }),
  created() {
    axios({
      method: "GET",
      url: "https://assessment-api.test.roo.cloud/v1/categories"
    })
      .then(({ data }) => {
        let temp = [...data.categories]
        let res = []
        temp[0].indent = 0
        res.push(temp[0])
        temp.splice(0, 1)
        let w = {}
        w.root = 0
        let i = 0
        while(temp.length){
          console.log(i, w,res, temp, 'wer\n')
          let p = res.findIndex(e=>e.slug===temp[i]['parent_slug'])
          if(p>-1){
            temp[i].indent = res[p].indent +1
            res.splice(p+1, 0, temp[i])
            temp.splice(i, 1)
          }
          else{
            i++
          }
          if(i>=temp.length){
            i = 0
          }
        }
        this.cats = res
      })
      .catch(e => console.log(e));
  }
};
</script>

<style>
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.title {
  font-family: "Quicksand", "Source Sans Pro", -apple-system, BlinkMacSystemFont,
    "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif;
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}
</style>
