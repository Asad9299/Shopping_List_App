<script setup>
import { computed, ref } from 'vue'

const header = ref('Shopping List App')

const editing = ref(false);

const isPurchased = ref(false);
const items = ref([
  // { id: 1, label: "10 party hats", isPurchased: true, itemHighPriority: false },
  // { id: 2, label: "2 board games", isPurchased: true, itemHighPriority: false },
  // { id: 3, label: "20 cups", isPurchased: false, itemHighPriority: true }
]);

const itemtoAdd = ref("");

const itemHighPriority = ref('');


const addAnItem = () => {
  items.value.push({ id: items.value.length + 1, label: itemtoAdd.value, itemHighPriority: itemHighPriority.value });

  itemtoAdd.value = '';
  itemHighPriority.value = '';
}

const doEdit = (e) => {
  editing.value = e;
  itemtoAdd.value = '';
  itemHighPriority.value = '';
}

const togglePruchased = (item) => {
  item.isPurchased = !item.isPurchased;
}

const latestItems = computed(() => {
  return [...items.value].reverse();
});

</script>

<template>
  <div class="header">
    <h1>{{ header }}</h1>
    <button class="btn" v-if="editing" @click="doEdit(false)">
      Cancel
    </button>

    <button v-else class="btn btn-primary" @click="doEdit(true)">
      Add Item
    </button>

  </div>

  <form class="add-item-form" @submit.prevent="addAnItem" v-if="editing">

    <input type="text" placeholder="Add an Item..." v-model.trim="itemtoAdd" />

    <label>
      Priority:
      <input type="checkbox" v-model="itemHighPriority">High Priority
    </label>

    <button class="btn btn-primary" :disabled="itemtoAdd.length < 3">
      Save Item
    </button>

  </form>

  <ul>
    <li v-for="item in  latestItems" :key="item.id" class="static-class" :class="{
      strikeout: item.isPurchased,
      priority: item.itemHighPriority,
    }" @click="togglePruchased(item)">
      {{ item.label }}
    </li>
  </ul>

  <div v-if="!items.length">
    <span>Nothing to see here.</span>
  </div>
</template>