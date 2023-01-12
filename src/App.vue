<script>
import { defineComponent } from 'vue'

export default defineComponent({
  name: 'App',
  components: {
  },
  data: {
    intervalID: null,
    URL: 'https://api.javascripttutorial.net/v1/quotes/?page=${page}&limit=${limit}',
    currentPage: 1,
    limit: 10,
    totalItems:0
  },
  methods: {
    displayQuotes: function (quotes){
      var quotesDiv = document.querySelector('.quotes');
      quotes.forEach(function(elem){
        var block = document.createElement('blockquote');
        block.classList.add('quote');
        var html = '<span>' + elem.id + '</span>';
        html += elem.quote;
        html += '<footer>' + elem.author + '</footer>';
        block.innerHTML = html;
        quotesDiv.appendChild(block);
      });
    },
    loadQuotes: function(){
      console.log('calling loadQuotes');
      var url = URL.replace('${page}', currentPage).replace('${limit}', limit);
      console.log('url', url);
      var oReq = new XMLHttpRequest();
      var that = this;
      oReq.addEventListener('load', function () {
        console.log('returning', this.responseText);
        var json = JSON.parse(this.responseText);
        displayQuotes(json.data);  
        totalItems = json.total;  
        currentPage++;
        intervalID = null;
      });
      oReq.addEventListener('error', function () {
        console.error('API not working!');
        intervalID = null;
      });
      oReq.open('GET', url);
      oReq.send();
    }
  }
})
</script>

<template>
  <div id="pcc-infinite">
  
  </div>
</template>

<style>

</style>
