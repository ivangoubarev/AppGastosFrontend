<template>
  <div>
    <h1>Bienvenido a la App de gastos</h1>
    <form @submit.prevent="addExpense">
      <div>
        <label for="fecha">Fecha: </label>
        <input type="date" v-model="newExpense.fecha" required />
      </div>
      <div>
        <label for="descripcion">Descripcion: </label>
        <input type="text" v-model="newExpense.descripcion" placeholder="Descripción" required />
      </div>
      <div>
        <label for="cantidad">Cantidad: </label>
        <input type="number" step="any" v-model="newExpense.cantidad" placeholder="Cantidad" required />
      </div>
      <button type="submit">Añadir Gasto</button>
    </form>
    <table>
      <tr>
        <th>Fecha</th>
        <th>Descripción</th>
        <th>Cantidad</th>
      </tr>
      <tr v-for="expense in expenses" :key="expense.id">
        <td>{{ formatDate(expense.fecha) }}</td>
        <td>{{ expense.descripcion }}</td>
        <td>{{ formatCurrency(expense.cantidad) }}</td>
      </tr>
    </table>
  </div>
</template>

<script>
import axios from 'axios'
import { format } from 'date-fns'

export default {
  name: 'App',
  data() {
    return {
      newExpense: {
        fecha: '',
        descripcion: '',
        cantidad: ''
      },
      expenses: []
    };
  },
  mounted() {
    axios.get('http://localhost:8080/api/gastos')
    .then(response => {
      this.expenses = response.data;
    })
    .catch(error => {
      console.error('Error al obtener los gastos: ', error);
    })
  },
  methods: {
    addExpense() {
      if (this.newExpense.fecha && this.newExpense.descripcion && this.newExpense.cantidad) {
        axios.post('http://localhost:8080/api/gastos', this.newExpense)
          .then(response => {
            console.log('Gasto añadido:', response.data);
            this.expenses.push(response.data);
            this.newExpense = {
              fecha: '',
              descripcion: '',
              cantidad: '',
            }
          })
          .catch(error => {
            console.error('Error al añadir el gasto:', error);
          })
      }
    },
    formatCurrency(value) {
      return new Intl.NumberFormat('es-ES', { style: 'currency', currency: 'EUR' }).format(value);
    },
    formatDate(dateString) {
      const date = new Date(dateString);
      return format(date, 'dd/MM/yyyy');
    }
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  text-align: center;
  margin-top: 60px;
}

form {
  margin-bottom: 20px;
}

form div {
  margin-bottom: 10px;
}

button {
  padding: 10px 20px;
  font-size: 16px;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  margin: 10px 0;
  padding: 10px;
  border: 1px solid #ddd;
}

table {
    width: 100%;
    border-collapse: collapse;
    margin-bottom: 20px;
}

th, td {
    padding: 10px;
    text-align: left;
}

th {
    background-color: #007bff;
    color: #fff;
}

tr:nth-child(even) {
    background-color: #f2f2f2;
}

tr:hover {
    background-color: #ddd;
}
</style>
