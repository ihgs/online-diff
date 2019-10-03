<template>
  <div>
    <div v-if="showdiff">
      <vue-code-diff :old-string="oldStr" :new-string="newStr" :context="10" outputFormat="side-by-side"/>
    </div>
  </div>
</template>
 

<script>
import vueCodeDiff from 'vue-code-diff'
import axiosBase from 'axios'

const axios = axiosBase.create({ headers:{'Content-Type': 'text/plain'}, transformResponse: (res) => {
            
            return res;
        }})
const diff_url = () => {
  const value = window.location.href.match(new RegExp("[?&]diff=(.*?)\\.{3}(.*?)(&|$|#)"));
  if (value == null) return '';
  return [decodeURIComponent(value[1]), decodeURIComponent(value[2])];
}
export default {
  name: 'app',
  components: {vueCodeDiff},
  data(){
    return {
      oldStr: null,
      newStr: null
    }
  },
  created: function(){
    const urls = diff_url()
    const base = 'https://gist.githubusercontent.com/'
    axios.get(base + urls[0]).then( (response) => {
      this.oldStr = response.data
    })
    axios.get(base + urls[1]).then( (response) =>{
      this.newStr = response.data
    })
  },
  computed: {
    showdiff: function(){
      return this.oldStr && this.newStr 
    }
  }
}


</script>

