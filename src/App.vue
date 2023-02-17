<script setup>
import { computed, ref } from 'vue';
import './components/main.css';
const header = ref('Daily Task App');
const editing = ref(false);
const plans = ref([
  {
    id: 1,
    title: "Shopping",
    completed: true,
    Important: false
  },
  {
    id: 2,
    title: "Play Chess",
    completed: true,
    Important: false
  },
  {
    id: 3,
    title: "Start Project",
    completed: true,
    Important: false
  },
  {
    id: 4,
    title: "Complete Project",
    completed: true,
    Important: true
  }
])
const newPlans = ref('');
const newPlansImportance = ref(false);
const savePlans = () => {
  plans.value.push({
    id: plans.value.length + 1,
    title: newPlans.value,
    Important: newPlansImportance.value,
  });
  newPlans.value = "";
  newPlansImportance.value = "";
};
const doEdit = (e) => {
  editing.value = e;
  newPlans.value = "";
  newPlansImportance.value = "";
};
const toggleReminder = (plan) => {
  plan.completed = !plan.completed;
}

const clear = ref(true);
const clearPlans = (c) => {
  if (window.confirm('Are you sure you want to clear all plans? This can\'t be undo')) {
    clear.value = c;
    plans.value = [];
  }
};

const clearCompleted = ref(false)
const filteredPlans = computed(() => {
  return clearCompleted.value ? plans.value.filter((p) => 
  !p.completed) : plans.value
})
</script>

<template>
  <div class="header">
    <h1>{{ header }}</h1>
    <button v-if="editing" class="btn" @click="doEdit(false)">Cancel</button>
    <button v-else class="btn btn-primary" @click="doEdit(true)">
      Add Plans
    </button>
  </div>

<form class="add-item-form" v-if="editing" @submit.prevent="savePlans">
  <input v-model.trim="newPlans" type="text" placeholder="Enter Plans" />
  <label>
      <input type="checkbox" v-model="newPlansImportance" />
      Importance
  </label>
  <br />
  <button :disabled="newPlans.length < 5" class="btn btn-primary">
  Save Plans
  </button>
</form>

<ul>
  <li
  v-for="(plan, index) in filteredPlans"
  @click="toggleReminder(plan)"
  :key="plan.id"
  class="static-class"
  :class="{
    strikeout: plan.completed,
    important: plan.Important,
  }"
  >
  {{ plan.title }}</li>
</ul>
<p v-if="!plans.length">No Plans Schedule Today</p>
<div class="filteringPlans"> 
  <button class="btn btn-primary" @click="clearPlans()">
  Clear All
</button>
<button class="btn btn-primary" @click="clearCompleted = !clearCompleted">
    {{ clearCompleted ? 'Undo' : 'Delete' }}
  </button>
</div>
</template>