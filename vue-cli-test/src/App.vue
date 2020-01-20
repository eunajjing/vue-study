<template>
  <div>
    <div>{{str}}</div>
    <!-- 리액트와 마찬가지로, 최상위 태그는 단 하나여야 한다 -->
    <app-header v-bind:propsdata="str" v-on:renew="renewStr" />
  </div>
</template>

<script>
import AppHeader from "./components/AppHeader.vue";
// vue라는 확장자까지 붙여주기를 권고한다

export default {
  data: function() {
    // 반드시 함수를 반환해야 한다
    return {
      str: "Header"
    };
  },
  components: {
    "app-header": AppHeader
  },
  methods: {
    renewStr: function() {
      this.str = "hi";
    }
  }
};
</script>

<style>
</style>