<template>
    <div>
      <h2>Respuesta del Servidor</h2>
      <div v-if="response">
        <h3>Matriz Rotada:</h3>
        <div v-if="response.rotatedMatrix">
          <p v-for="(row, index) in response.rotatedMatrix" :key="index">
            {{ row }}
          </p>
        </div>
        <h3>Estadísticas:</h3>
        <div v-if="response.stats">
          <p><strong>Máximo:</strong> {{ response.stats.max }}</p>
          <p><strong>Mínimo:</strong> {{ response.stats.min }}</p>
          <p><strong>Media:</strong> {{ response.stats.mean }}</p>
          <p><strong>Suma:</strong> {{ response.stats.sum }}</p>
          <p><strong>Es Diagonal:</strong> {{ response.stats.isDiagonal ? 'Sí' : 'No' }}</p>
        </div>
      </div>
      <div v-else>
        <p>Aún no se ha realizado la solicitud.</p>
      </div>
      
      <textarea v-model="matrixInput" rows="3" cols="30" placeholder="Ingrese la matriz como JSON"></textarea>
      <button @click="rotateMatrix">Rotar Matriz</button>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  
  export default {
    data() {
      return {
        matrixInput: '',
        response: null, 
      };
    },
    methods: {
      rotateMatrix() {
        try {
          const matrix = JSON.parse(this.matrixInput.trim());
          axios.post('http://localhost:3100/rotate-matrix', { matrix })
            .then(response => {
              this.response = response.data; // Actualiza la respuesta para mostrarla en el frontend
            })
            .catch(error => {
              console.error('Error al rotar la matriz:', error);
            });
        } catch (error) {
          console.error('Error de formato JSON:', error);
        }
      }
    }
  };
  </script>
  