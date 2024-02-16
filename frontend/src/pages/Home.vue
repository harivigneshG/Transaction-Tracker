<template>
  <div>

    <nav class="bg-white border-gray-200 dark:bg-red-500">
    <div class="flex flex-wrap justify-between items-center mx-auto max-w-screen-xl p-3">
        <a href="#" class="flex items-center space-x-3 rtl:space-x-reverse">
            <span class="self-center text-2xl font-semibold whitespace-nowrap dark:text-white">Expenses Tracker</span>
        </a>
        <div class="flex items-center space-x-6 rtl:space-x-reverse">
            <a href="#" class="text-sm  text-white dark:text-white-500 hover:text-red-100"> Hello, {{ session.user }}</a>
        </div>
    </div>
    </nav>



  
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
  



  
 
    
    
    <Todo  @transactionSubmitted="handlesubmit"/>

  </div>

  
</template>

<script setup>
import Todo from './Todo-adder.vue'
import { session } from '../data/session'
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

const handlesubmit=(data)=>{
  if(data.val){
    console.log(data.val);
    action.reload()
  }
   
}
</script>
