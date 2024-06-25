<template>
  <v-card>
    <v-card-title>{{ column.title }}</v-card-title>
    <v-card-text>
      <v-list>
        <v-list-item
          v-for="(card, cardIndex) in column.cards"
          :key="card.id"
          :draggable="true"
          @dragstart="onDragStart(card)"
          @dragover.prevent
          @drop="onDrop"
        >
          <template v-if="!card.editing">
            <v-list-item-content @click="editCard(card, true)">
              <v-list-item-title>{{ card.title }}</v-list-item-title>
              <v-list-item-subtitle>{{ card.description }}</v-list-item-subtitle>
            </v-list-item-content>
          </template>
          <template v-else>
            <v-list-item-content>
              <v-text-field v-model="card.title" label="Title" outlined dense></v-text-field>
              <v-textarea v-model="card.description" label="Description" outlined dense></v-textarea>
              <v-btn @click="saveEdit(card)">Save</v-btn>
              <v-btn @click="editCard(card, false)">Cancel</v-btn>
            </v-list-item-content>
          </template>
          <v-divider v-if="cardIndex < column.cards.length - 1"></v-divider>
        </v-list-item>
      </v-list>
      <v-text-field v-model="newTaskTitle" label="New Task Title" outlined dense></v-text-field>
      <v-btn @click="addTask">Add Task</v-btn>
    </v-card-text>
    <v-card-actions>
      <v-btn color="error" @click="removeColumn">Remove Column</v-btn>
    </v-card-actions>
  </v-card>
</template>

<script setup lang="ts">
import { ref } from 'vue';
import Cards from './Cards.vue';

const props = defineProps({
  column: Object,
  columnIndex: Number
});
const emit = defineEmits(['remove-column', 'add-task', 'drag-start', 'drop', 'edit-card']);

const newTaskTitle = ref('');

const onDragStart = (card) => {
  emit('drag-start', card, props.column);
};

const onDrop = () => {
  emit('drop', props.column);
};

const editCard = (card, isEditing) => {
  emit('edit-card', card, isEditing);
};

const saveEdit = (card) => {
  editCard(card, false);
};

const addTask = () => {
  emit('add-task', props.columnIndex, newTaskTitle.value.trim());
  newTaskTitle.value = '';
};

const removeColumn = () => {
  emit('remove-column', props.columnIndex);
};
</script>
