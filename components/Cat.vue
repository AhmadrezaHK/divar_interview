<template>
<div :style="{paddingLeft: `${indent*20}px`}">
  <button @click="getschema(slug)" class="d-block">
    {{ cat.title }}
  </button>
    <div v-if="form.length" class="border p-2">
      <template v-for="f in form">
        <template v-if="f.type==='string'">
          <input :key="f.title" type="text" :placeholder="f.title" class="d-block">
        </template>
        <template v-else-if="f.type==='boolean'">
          <input :key="f.title" type="radio" >
        </template>
      </template>
    </div>
    <!-- <slot v-if="$slot" /> -->
</div>
</template>
<script>
import axios from "axios";

export default {
  data:()=>({
    form:[]
  }),
  props: {
    slug: {
      type: String,
      required: true
    },
    indent: {
      type: Number,
      required: true
    }
    ,
    cat: {
      type: Object,
      required: true
    }
  },
  methods:{
    getschema(slug){
      axios({
      method: "GET",
      url: `https://assessment-api.test.roo.cloud/v1/categories/${slug}`
    })
      .then(({ data }) => {
        for(const i in data.json_schema.properties){
          if(['string', 'boolean'].includes(data.json_schema.properties[i].type)){
            this.form.push(data.json_schema.properties[i])
          }
        }
      })
      .catch(e => console.log(e));
      
    }
  }
};
</script>

<style></style>
