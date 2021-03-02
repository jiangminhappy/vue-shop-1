<template>
  <div class="recommend">
    <div v-for="item in recommend" class="item" :key="item.link">
      <a :href="item.link">
        <img class="item-img" :src="item.image" alt="">
        <div>{{item.title}}</div>
      </a>
    </div>
  </div>
</template>

<script>
export default {
  name: 'RecommendView',
  props: {
    recommend: {
      type: Array,
      default() {
        return []
      }
    }
  }
}
</script>

<style scoped>
  .recommend {
    display: flex;
    width: 100%;
    text-align: center;
    font-size: 12px;
    padding: 10px 0 20px;
    border-bottom: 8px solid #eee;
  }
  .item {
    flex: 1;
  }

  .item-img {
    width: 70px;
    height: 70px;
    margin-bottom: 10px;
  }
</style>