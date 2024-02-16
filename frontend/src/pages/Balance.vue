<template>
    <div class="mx-6 flex flex-row space-x-4">
        <div class="relative flex flex-col mt-6 text-gray-700 bg-white shadow-md bg-clip-border rounded-xl w-80">
  <div class="p-4">
    <h5 class="block mb-2 font-sans text-xl antialiased font-light leading-snug tracking-normal text-blue-gray-900">
     Your Balance
    </h5>
    <p class="block font-sans antialiased font-bold leading-relaxed text-2xl" v-if="action.data">
        ₹ {{ action.data.total}}
    </p>
  </div>
</div>


<div class="relative flex flex-col mt-6 text-gray-700 bg-white shadow-md bg-clip-border rounded-xl w-80">
  <div class="p-4">
    <h5 class="block mb-2 font-sans text-xl antialiased font-light leading-snug tracking-normal text-blue-gray-900">
     Income
    </h5>
    <p class="block font-sans antialiased font-bold leading-relaxed text-2xl text-green-500" v-if="action.data">
        ₹ {{ action.data.incomeTotal}}
    </p>
  </div>
</div>


<div class="relative flex flex-col mt-6 text-gray-700 bg-white shadow-md bg-clip-border rounded-xl w-80">
  <div class="p-4">
    <h5 class="block mb-2 font-sans text-xl antialiased font-light leading-snug tracking-normal text-blue-gray-900">
     Expenses
    </h5>
    <p class="block font-sans antialiased font-bold leading-relaxed text-2xl text-red-600" v-if="action.data">
        ₹ {{ action.data.expenseTotal}}
    </p>
  </div>
</div>
    </div> 
  
</template>

<script setup>
import { createListResource } from 'frappe-ui'



const action= createListResource({
  'doctype' : 'Expense Tracker',
  'fields' : ['name', 'transaction_name', 'type', 'amount' ],
  transform(action){

    return action.reduce((acc, t) => {
    if (t.type === 'Expense') {
      acc.expenseTotal += t.amount;
    } else if (t.type === 'Income') {
      acc.incomeTotal += t.amount;
    }

    acc.total = acc.incomeTotal -  acc.expenseTotal;
    return acc;
  }, { expenseTotal: 0, incomeTotal: 0, total: 0 });
  }

})

action.reload()


// console.log(action)

//   const expense_amount = computed(() => {

//     if (!action.list.loading || action.data){

//     return action.data
//       .filter((t) => t.type === "Expense")
//       .reduce((acc, t) => {
//         return acc + t.amount; // assuming 'amount' is the field you want to accumulate
//       }, 0);
//     }
//   });

// /

</script>