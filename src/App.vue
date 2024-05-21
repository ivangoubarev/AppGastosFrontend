<template>
  <div>
    <h1>Bienvenido a la App de gastos</h1>
    <form @submit.prevent="addExpense">
      <div>
        <label for ="date">Fecha: </label>
        <input type="date" v-model="newExpense.date" required />
      </div>
      <div>
        <label for ="description">Descripcion: </label>
        <input type="text" v-model="newExpense.description" placeholder="Descripción" required />
      </div>
      <div>
        <label for ="amount">Cantidad: </label>
        <input type="number" step="any" v-model="newExpense.amount" placeholder="Cantidad" required />
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
        <td>{{ expense.date }}</td>
        <td>{{ expense.description }}</td>
        <td>{{ formatCurrency(expense.amount) }}</td>
      </tr>
    </table>
    <!-- <ul>
      <li v-for="expense in expenses" :key="expense.id">
        {{ expense.date }} - {{ expense.description }}: {{ formatCurrency(expense.amount) }}
      </li>
    </ul> -->
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      newExpense: {
        date: '',
        description: '',
        amount: ''
      },
      expenses: []
    };
  },
  methods: {
    addExpense() {
      if (this.newExpense.date && this.newExpense.description && this.newExpense.amount) {
        const id = Date.now()
        this.expenses.push({ ...this.newExpense, id});
        this.newExpense.date = '';
        this.newExpense.description = '';
        this.newExpense.amount = '';
      }
    },
    formatCurrency(value) {
      return new Intl.NumberFormat('es-ES', { style: 'currency', currency: 'EUR' }).format(value);
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
