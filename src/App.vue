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
    URL: 'https://api.javascripttutorial.net/v1/quotes/?page=${page}&limit=${limit}',
    currentPage: 1,
    limit: 10,
    totalItems: 0,
    items: []
  },
  methods: {
    handleScroll: function (){
      /*
      console.log('handleScroll currentPage', currentPage);
      console.log('handleScroll totalItems', totalItems);
      console.log('handleScroll(currentPage - 1) * 10', (currentPage - 1) * 10);
      console.log('intervalID', intervalID);
      */
      if (document.documentElement.scrollTop + document.documentElement.clientHeight >= document.documentElement.scrollHeight - 5 && (this.currentPage - 1) * 10 < this.totalItems){
        if (this.intervalID === null){
          this.intervalID = setTimeout(this.loadQuotes, 500);
        }
      }
    },
    loadQuotes: function () {
      console.log('calling loadQuotes')
      var url = this.URL.replace('${page}', this.currentPage).replace('${limit}', this.limit)
      console.log('url', url)
      var oReq = new XMLHttpRequest()
      var that = this
      oReq.addEventListener('load', function () {
        //console.log('returning', this.responseText)
        var json = JSON.parse(this.responseText)
        that.totalItems = json.total;
        that.currentPage++;
        console.log('that.items', that.items);
        that.items.push(...json.data);
        //this.displayQuotes(json.data)
        that.intervalID = null
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
                href="http://54.252.3.209:8000/your-council/city-planning-and-reporting/our-strategic-priorities/healthy-harbour/te-awarua-o-porirua-harbour-faqs/"
                >#{{i}} {{ item.quote }}</a
              >
            </h4>
            <p>Contact us if you have more questions about our harbour.</p>
            <ul class="breadcrumb hidden-print">
              <li><a href="http://54.252.3.209:8000/">Porirua City</a></li>
              <li class="active title-tereo">Te Awarua-o-Porirua Harbour FAQs</li>
            </ul>
            <p class="pcc-promo-j-updated mb-0">Last updated: 2 August 2019</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style></style>
