<template>
  <div class="home">
    <h1>商品検索(タグ)<br>localStrage</h1>

    <p><input type="text" v-model="keyword"></p>
    <div class="search__wrapper">
      <ul>
        <li v-for="(p, index) in searchFiltered" :key="index">
          {{ p.name }}

        </li>
      </ul>
    </div><!-- /.search__wrapper -->
  </div>
</template>

<script>
export default {
  name: 'Home',
  data() {
    return {
      searchProducts: [],
      keyword: '',
    }
  },
  computed: {
    //and検索
    searchFiltered() {
      return this.searchProducts.filter( searchProduct => {
        if(this.keyword) {
          //検索文字列をspaceで区切ったものを配列にし
          //全ての検索文字列にマッチしたものだけtrueで返す
          return this.keyword.toLowerCase()
            .split(/\s+/)
            .map( q => searchProduct.selectedTags.includes(q) || searchProduct.name.toLowerCase().indexOf(q) > -1 )
            .every( result => result === true );
            //everyの代わりにsome()を使うと、一つでも条件がマッチしたものでtrueを返す
        }
      })
    }
    // searchFiltered() {
    //   return this.searchProducts.filter(searchProduct => {
    //     if(this.keyword) {
    //       return searchProduct.selectedTags.includes(this.keyword);
    //     }
    //   })
    // }
  },
  mounted() {
    // console.log(this.selectedTags);
    if (localStorage.getItem('products')) {
      try {
        this.searchProducts = JSON.parse(localStorage.getItem('products'));
      } catch(e) {
        localStorage.removeItem('products');
      }
    }
  },
}
</script>


<style scoped>
input {
  font-size: 16px;
}
.search__wrapper {
  background-color: rgb(207, 244, 253);
  text-align: left;
  padding: 40px 60px;
}
</style>
