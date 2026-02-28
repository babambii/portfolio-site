<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png">
    <HelloWorld msg="Welcome to Your Vue.js + TypeScript App"/>
    <div class="counter">
      <p>カウント: {{ count }}</p>
      <button @click="increment">カウントアップ</button>
    </div>
  </div>
</template>

<script lang="ts">
import Vue from 'vue';
import HelloWorld from '@/components/HelloWorld.vue';

export default Vue.extend({
  name: 'HomeView',
  components: {
    HelloWorld,
  },
  data() {
    return {
      count: 0,
    };
  },
  methods: {
    increment(): void {
      this.count++;
    },
  },
});
</script>
