<template>
  <Grid v-for="_ in range"/>
  <button @click="add">增加一个表格</button>
</template>

<script>
import Grid from './components/Grid.vue';

export default {
  name: 'App',
  components: {
    Grid
  },
  data() {
    return {
      range: Array(1)
    };
  },
  methods: {
    add() {
      this.range.push(0);
    }
  }
};
</script>
