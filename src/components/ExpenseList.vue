<template>
  <div class="ui row">
    <div class="four wide column">
      <expense-input
        @addExpense="addExpense">
      </expense-input>
    </div>
    <div id="item-container" class="eleven wide column">  
      <expense-item
        v-for="item in expenseList"
        :key="item.id"
        :item="item"
        @deleteExpense="deleteExpense">
      </expense-item>
    </div>
  </div>
</template>

<script>
  import ExpenseItem from './ExpenseItem'
  import ExpenseInput from './ExpenseInput'
export default {
  name: "ExpenseList",
  components: {
    ExpenseItem,
    ExpenseInput
  },
  data() {
    return {
      expenseList: [{
        id: 1,
        date: '2017-11-01',
        expense: 13
      }]
    }
  },
  methods: {
    addExpense: function(payload) {
      if (payload.expense) {
        this.expenseList.push(payload);
        this.$emit('updateChart', this.expenseList);
      }  
    },
    deleteExpense: function(id) {
      let index = this.expenseList.map(function(item) {
        return item.id
      }).indexOf(id);

      this.expenseList.splice(index, 1);
      this.$emit('updateChart', this.expenseList);
    }

  }
}
</script>

<style scoped>
  #item-container {
    height: 216px;
    overflow-y: scroll;
  }
</style>
