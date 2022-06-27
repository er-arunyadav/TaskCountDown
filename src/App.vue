<template>
  <div>
    <div v-if="loader.showForm"
      class="relative max-w-md mx-auto md:max-w-2xl mt-6 min-w-0 break-words bg-white w-full mb-6 rounded-xl mt-16 ">
      <div class="px-6">
        <div class="mb-4">
          <label class="block text-gray-700 text-sm font-bold mb-2 " for="username">
            Task
          </label>
          <input v-model="task"
            class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
            id="username" type="text" placeholder="Enter your task">

          <p class="text-red-700	" v-if="error.message.length > 0">{{ error.message['task'] }}</p>
        </div>
        <div class="mb-4">
          <label class="block text-gray-700 text-sm font-bold mb-2" for="username">
            Choose date
          </label>
          <Datepicker v-model="date" :is24="false" :minDate="new Date()" :minTime="{ minutes: 10 }" showNowButton>
            <template #am-pm-button="{ toggle, value }">
              <button @click="toggle">{{ value }}</button>
            </template>
          </Datepicker>
          <p class="text-red-700	" v-if="error.message.length > 0">{{ error.message['date'] }}</p>
        </div>
        <div class="mb-4">
          <button @click="submit" class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded">
            Submit
          </button>
        </div>
      </div>
    </div>
    {{ task.value }}
    <div v-if="loader.showClock">
      <Counter :title="endDate.title" :endDate="endDate.formatDate" />
    </div>
  </div>
</template>
<script setup>
import { ref, reactive } from 'vue'
import Datepicker from '@vuepic/vue-datepicker';
import '@vuepic/vue-datepicker/dist/main.css'
import Counter from "@/components/Counter.vue";
const date = ref(new Date());
const task = ref('');
const endDate = reactive({
  formatDate: {},
  title: ''
});
const loader = reactive({
  showForm: true,
  showClock: false,
});
const error = reactive({
  message: [
    'task',
    'date'
  ]
});
const clearError = () => {
  return error.message = ['task', 'date']
}

const formattedDate = (date) => {
  const year = date.getFullYear()
  const month = date.getMonth()
  const day = date.getDate()
  const hours = date.getHours()
  const minutes = date.getMinutes()
  const seconds = date.getSeconds()
  const milliseconds = date.getMilliseconds()
  return {
    year,
    month,
    day,
    hours,
    minutes,
    seconds,
    milliseconds
  }
}

const submit = () => {
  clearError()
  if (task.value == '') return error.message['task'] = 'Task is mandtory';
  if (date.value == '') return error.message['date'] = 'Date is mandtory';
  const d = new Date(date.value);
  endDate.formatDate = formattedDate(d);
  endDate.title = task.value
  // console.log(endDate);
  loader.showForm = false;
  loader.showClock = true;

}

</script>
