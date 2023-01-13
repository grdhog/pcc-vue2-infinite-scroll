<script>
import { defineComponent } from 'vue'

export default defineComponent({
  name: 'App',
  components: {},
  mounted: function () {
    console.log('mounted')
    this.loadQuotes();
    window.addEventListener('scroll', this.handleScroll);
  },
  data: {
    intervalID: null,
    URL: 'http://54.252.3.209:8000/search-json/',
    currentPage: 1,
    limit: 10,
    total: 0,
    items: []
  },
  methods: {
    handleScroll: function (){
      /*
      console.log('handleScroll currentPage', currentPage);
      console.log('handleScroll total', total);
      console.log('handleScroll(currentPage - 1) * 10', (currentPage - 1) * 10);
      console.log('intervalID', intervalID);
      */
      if (document.documentElement.scrollTop + document.documentElement.clientHeight >= document.documentElement.scrollHeight - 5 && (this.currentPage - 1) * 10 < this.total){
        if (this.intervalID === null){
          this.intervalID = setTimeout(this.loadQuotes, 500);
        }
      }
    },
    loadQuotes: function () {
      console.log('calling loadQuotes')
      //var url = this.URL.replace('${page}', this.currentPage).replace('${limit}', this.limit)
      var url = this.URL + '?q=Porirua&page=' + this.currentPage;      
      console.log('url', url)
      var oReq = new XMLHttpRequest()
      var that = this
      oReq.addEventListener('load', function () {
        //console.log('returning', this.responseText)
        var json = JSON.parse(this.responseText)
        that.total = json.total;
        that.currentPage++;
        console.log('that.items', that.items);
        that.items.push(...json.results);
        that.intervalID = null;
      })
      oReq.addEventListener('error', function () {
        console.error('API not working!')
        intervalID = null
      })
      oReq.open('GET', url)
      oReq.send()
    },
  },
})
</script>

<template>
  <div id="pcc-infinite">
    <div class="container">
      <div class="row">
        <div id="search-results" class="col-sm-12">
          <div v-for="(item, i) in items" class="pcc-promo-j">
            <h4>
              <a
                class="pcc-promo-j-link"
                :href="item.url"
                >#{{i}} {{ item.title }}</a
              >
            </h4>
            <p>{{ item.search_description }}</p>
            <ul class="breadcrumb hidden-print">
              <li><a href="http://54.252.3.209:8000/">Porirua City</a></li>
              <li class="active title-tereo">Te Awarua-o-Porirua Harbour FAQs</li>
            </ul>
            <p class="pcc-promo-j-updated mb-0">Last updated: {{ item.latest_revision_created_at }}</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style></style>
