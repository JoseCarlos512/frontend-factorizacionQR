<template>
  <div id="app">
    <MatrixStats />
    <QRDecomposition />
  </div>
</template>

<script>
import MatrixStats from './components/MatrixStats.vue';
import QRDecomposition from './components/QRDecomposition.vue';

export default {
  name: 'App',
  components: {
    MatrixStats,
    QRDecomposition
  }
};
</script>

<style>
#app {
  font-family: 'Arial', sans-serif;
  text-align: center;
  margin-top: 20px;
}
</style>
