<template>
  <div class="search">
      <div class="logo">
          <img src="../../images/bookstore.png"  />
      </div>
      <div class="search_form">
          <form  v-on:submit.prevent="query()" class="form-wrapper cf">
              <input name="keyword" type="text" v-model="keyword"  placeholder="输入书名..." required/>
              <button type="submit" v-on:click.prevent="query">搜索</button>
          </form>
      </div>
      <div class="showloading">{{loadingText}}</div>
      <div class="alert alert-info" v-if="resultItems.length == 0 && loadingText ==''"><strong>提示：</strong>搜索文字尽量匹配完整书名.</div>
      <div class="search_results" v-if="resultItems.length > 0">
          <ul>
              <li v-for="item in resultItems">
                  <div>
                      <a v-bind:href="item.detailUrl" target="_blank"><img v-bind:src="item.imageUrl" /></a>
                  </div>
                  <div class="shop">{{item.shop.name}}</div>
                  <div class="name"><a v-bind:href="item.detailUrl" target="_blank">{{item.name}}</a></div>
                  <div class="price">￥{{item.sellPrice}}</div>
              </li>
          </ul>
      </div>
      <div style="clear:both" />
  </div>
</template>
<script>
export default {
  name: 'search',
  data: function() {
    return {
        keyword: '',
        loadingText: '',
        resultItems: []
    }
  },
  methods: {
      query: function() {
          if($.trim(this.keyword) !== '') {
              this.loadingText = "查询中...";
              this.resultItems = [];
              this.$http.post('search?keyword=' + encodeURI(this.keyword)).then(function(res) {
                  console.log(res.data);
                  this.resultItems = res.data;
                  this.loadingText = "";
                  if(res.data.length == 0) {
                      this.loadingText = "没有找到任何记录。";
                  }
              });
          }
      }
  }
}
</script>

