<template>
  <div>
    <Header/>
  
    <section class="page">

      <div class="row" v-if="article">
         <div class="col span-3-of-3">
           <div class="article">
        {{article.fields.abstract}}
           </div>
         </div>
      </div>

    </section>

    <Footer/>
  </div>
  
</template>

<script>

import Header from '~/components/Header.vue'
import Footer from '~/components/Footer.vue'

const contentful = require('contentful')

const client = contentful.createClient({
  space: process.env.CTF_SPACE_ID,
  accessToken: process.env.CTF_CDA_ACCESS_TOKEN
})

export default {
  components: {
    Header,
    Footer
  },
  data(){
    return {
      article: null
    }
  },
  asyncData(context){
    let query = context.params.page.replace(/-/g, ' ')
    return client.getEntries(
      {
        'content_type': 'article',
        'fields.title[match]': query
      }
    )
    .then((res) => {
      return {
      article: res.items[0]
      }
    }).catch(console.error)
  }

}
</script>

<style>
 .article {
   background-color: #fff;
   width: 85%;
   padding: 30px;
 }
 .page {
   padding: 0px;
 }
</style>