<template>

<div class="flex flex-row-reverse mx-6 my-4">

  <Button icon-left="plus" @click="showDialog = true" > Add Transaction </Button>
  
       
  <Dialog v-model="showDialog">
  <template #body-title>
    <h3> Add New Transaction</h3>
  </template>
  <template #body-content>

      <div class="flex flex-col space-y-3">
        <Input
          required
          name="action"
          type="text"
          placeholder="Action Name"
          label="Action Name"
          v-model="state.action"
        />
        <Input
          required
          name="type"
          type="select"
          :options="['', 'Income', 'Expense']"
          label="Type"
          v-model="state.type"
        />

        <Input
          required
          name="amount"
          type="text"
          label="Amount"
          v-model="state.amount"
        />
</div>


  
    
  </template>
  <template #actions>
    <Button variant="solid" @click="submit">
      Add 
    </Button>
    <Button
      class="ml-2"
      @click="showDialog = false"
    >
      Close
    </Button>
  </template>
</Dialog>
</div>
  
   
   <div class="mx-4 w-1/3">
    <Card title="History">
      <div >
        <ul  >
          <li class="flex flex-row space-y-2 item-center justify-between" v-for="i in action_list.data" :key="i.name"> <a class="text-4xl text-gray-900 font-extralight"> {{ i.transaction_name }} </a>  <a :class="{ 'text-green-600': i.type === 'Income', 'text-red-600': i.type === 'Expense' }"> {{ i.type === 'Income' ? '+' : '-' }} {{ i.amount }}</a>
 </li>
        </ul>
      </div>
    

    </Card>

   </div>
 
  

    


</template>

<script setup>
import { ref } from 'vue'
import { Dialog , createListResource , Card } from 'frappe-ui'


const emit =defineEmits(['transactionSubmitted'])

const showDialog = ref(false)

const state = ref({
      action: '',
      type: '',
      amount: ''

    });


const action_list = createListResource({
  'doctype' : 'Expense Tracker',
  'fields' : ['name', 'transaction_name', 'type', 'amount' ]

})
action_list.reload()


    
const submit = ()=> {
    if (state.value.action != '' || state.value.type != "" ||  state.value.amount != ""){

      action_list.insert.submit({
        "transaction_name" : state.value.action , 
        "type" : state.value.type,
        "amount" : state.value.amount,
        onSuccess(){
          action_list.reload()
          showDialog.value = false;

        }
      })
      const data={
            val : true
          }
      emit('transactionSubmitted', data);
      showDialog.value = false;
    }

}

</script>