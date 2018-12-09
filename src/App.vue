<template>
  <div id="app" class="ui center aligned padded grid">
  <h1 class="ui ">Expense Chart</h1>
    <div class="ui row">
      <div class="fifteen wide column">
        <expense-charts :chart-data="datacollection"></expense-charts>
      </div>
    </div>
    <expense-list @updateChart="updateChart"></expense-list>
  </div>
</template>

<script>
import ExpenseList from './components/ExpenseList'
import ExpenseCharts from './components/ExpenseCharts'

export default {
  name: 'App',
  components: {
    ExpenseList,
    ExpenseCharts
  },
  data() {
    return {
      datacollection: {
        labels: ['2017-11-01'],
        datasets: [
          {
            label: "Expenses",
            backgroundColor: "#000000",
            borderColor: "#a51313",
            fill: false,
            data: ['13']
          }
        ]
      }
    }
  },
  methods: {
    updateChart: function(expenseData) {
      expenseData.sort((a,b) => {
        if (a.date < b.date) {
          return -1
        } else if (a.date > b.date) {
          return 1
        }
        return 0
      })

      let chartDataDate = [];
      let chartDataExpense = [];

      expenseData.map((e, i) => {
        if (i === 0) {
          chartDataDate.push(e.date);
          chartDataExpense.push(e.expense);
        } else if (chartDataDate.includes(e.date)) {
          let index = chartDataDate.indexOf(e.date);
          chartDataExpense[index] = parseInt(chartDataExpense[index]) + parseInt(e.expense);
        } else {
          chartDataDate.push(e.date);
          chartDataExpense.push(e.expense);
        }
      });

      this.datacollection = {
        labels: chartDataDate,
        datasets: [
          {
            label: "Expenses",
            backgroundColor: "#000000",
            fill: false,
            borderColor: "#a51313",
            data: chartDataExpense
          }
        ]
      }
    }
  }
}
</script>

<style>
  #app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    color: #2c3e50;
    width: 100vw;
  }

  body {
    overscroll-behavior-y: none;
  }
</style>
