<template>
    <div>
      <h2>QR Factorization</h2>
      <!-- Inputs para definir filas y columnas -->
    <div>
      <label>Filas:</label>
      <input type="number" v-model="rows" @input="createMatrixInputs">
      <label>Columnas:</label>
      <input type="number" v-model="cols" @input="createMatrixInputs">
    </div>

    <!-- Inputs generados dinámicamente para la matriz -->
    <div v-if="matrixInputs.length > 0">
      <div v-for="(row, rowIndex) in matrixInputs" :key="rowIndex">
        <div>
          <label v-for="(col, colIndex) in row" :key="colIndex">
            {{ `Valor para (${rowIndex + 1},${colIndex + 1}):` }}
            <input type="number" v-model="matrixInputs[rowIndex][colIndex]">
          </label>
        </div>
      </div>
      <button @click="factorizeMatrix">Factorize</button>
    </div>

    <div v-else>
      <p>Ingrese el número de filas y columnas.</p>
    </div>
  
      <!-- Mostrar resultados de la factorización -->
      <div v-if="qrResponse">
        <h3>Factorization Response</h3>
        <div v-if="qrResponse.Q && qrResponse.R">
          <h4>Q Matrix</h4>
          <div v-for="(row, index) in qrResponse.Q" :key="index">
            <p>{{ row }}</p>
          </div>
  
          <h4>R Matrix</h4>
          <div v-for="(row, index) in qrResponse.R" :key="index">
            <p>{{ row }}</p>
          </div>
  
          <h4>Statistics</h4>
          <div v-if="qrResponse.stats">
            <p><strong>Q Max:</strong> {{ qrResponse.stats.Q.max }}</p>
            <p><strong>Q Min:</strong> {{ qrResponse.stats.Q.min }}</p>
            <p><strong>Q Mean:</strong> {{ qrResponse.stats.Q.mean }}</p>
            <p><strong>Q Sum:</strong> {{ qrResponse.stats.Q.sum }}</p>
            <p><strong>Q Is Diagonal:</strong> {{ qrResponse.stats.Q.isDiagonal ? 'Yes' : 'No' }}</p>
  
            <p><strong>R Max:</strong> {{ qrResponse.stats.R.max }}</p>
            <p><strong>R Min:</strong> {{ qrResponse.stats.R.min }}</p>
            <p><strong>R Mean:</strong> {{ qrResponse.stats.R.mean }}</p>
            <p><strong>R Sum:</strong> {{ qrResponse.stats.R.sum }}</p>
            <p><strong>R Is Diagonal:</strong> {{ qrResponse.stats.R.isDiagonal ? 'Yes' : 'No' }}</p>
          </div>
        </div>
      </div>
  
      <div v-else>
        <p>No factorization results yet.</p>
      </div>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  
  export default {
    data() {
      return {
        rows: 0,
        cols: 0,
        matrixInputs: [],
        qrResponse: null,
      };
    },
    methods: {
      createMatrixInputs() {
        this.matrixInputs = [];
        for (let i = 0; i < this.rows; i++) {
          this.matrixInputs.push(new Array(this.cols).fill(''));
        }
      },
      factorizeMatrix() {
        try {
          const matrix = this.matrixInputs.map(row => row.map(Number));

          console.log(matrix)

          axios.post('http://localhost:3100/qr-factorization', { matrix })
            .then(response => {
              console.log('Factorization response:', response.data);
              this.qrResponse = response.data; // Actualizar la respuesta para mostrarla en el frontend
            })
            .catch(error => {
              console.error('Error factorizing matrix:', error);
            });
        } catch (error) {
          console.error('Invalid matrix input:', error);
        }
      }
    }
  };
  </script>