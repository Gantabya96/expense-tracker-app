<template>
  <div>
    <div class="mb-3">
      <button @click="filter = 'all'" class="btn btn-secondary me-2">All</button>
      <button @click="filter = 'Income'" class="btn btn-success me-2">Income</button>
      <button @click="filter = 'Expense'" class="btn btn-danger">Expense</button>
    </div>
    <table class="table table-bordered">
      <thead>
        <tr>
          <th>Title</th>
          <th>Amount</th>
          <th>Type</th>
          <th>Action</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(transaction, index) in filteredTransactions" :key="index">
          <td>{{ transaction.title }}</td>
          <td
            :class="{
              'fw-bold': transaction.amount >= 500,
              'text-success': transaction.type === 'INCOME',
              'text-danger': transaction.type === 'EXPENSE',
            }"
          >
            ${{ transaction.amount }}
          </td>
          <td>{{ transaction.type }}</td>
          <td>
            <button @click="$emit('delete-transaction', index)" class="btn btn-danger">
              Delete
            </button>
          </td>
        </tr>
      </tbody>
    </table>
    <p v-if="filteredTransactions.length === 0" class="text-center">
      No transactions recorded yet.
    </p>
  </div>
</template>

<script>
import { ref, computed } from 'vue'
export default {
  props: ['transactions'],
  emits: ['delete-transaction'],
  setup(props) {
    const filter = ref('all')

    const filteredTransactions = computed(() => {
      if (filter.value === 'all') return props.transactions
      return props.transactions.filter((t) => t.type === filter.value.toUpperCase())
    })

    return { filter, filteredTransactions }
  },
}
</script>
