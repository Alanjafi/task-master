<template>
  <v-container class="custom-background">
    <v-row>
      <v-col v-for="(column, columnIndex) in columns" :key="column.id" cols="4">
        <Column :column="column" :columnIndex="columnIndex" @remove-column="removeColumn" @add-task="addTask" @drag-start="onDragStart" @drop="onDrop" @edit-card="editCard" />
      </v-col>
    </v-row>
    <v-row>
      <v-col cols="4">
        <v-card class="custom-card"> 
          <v-card-title>Add New Column</v-card-title>
          <v-card-text>
            <v-text-field v-model="newColumnName" label="Column Name" outlined dense></v-text-field>
          </v-card-text>
          <v-card-actions>
            <v-btn color="primary" @click="addColumn">Add Column</v-btn>
          </v-card-actions>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script setup lang="ts">
import { ref } from 'vue';
import Column from './Column.vue';

interface Card {
  id: number;
  title: string;
  description: string;
  editing: boolean;
}

interface Column {
  id: number;
  title: string;
  cards: Card[];
}

const columns = ref<Column[]>([
  {
    id: 1,
    title: 'To Do',
    cards: [
      { id: 1, title: 'Task 1', description: 'Description 1', editing: false },
      { id: 2, title: 'Task 2', description: 'Description 2', editing: false },
    ],
  },
  {
    id: 2,
    title: 'Doing',
    cards: [
      { id: 3, title: 'Task 3', description: 'Description 3', editing: false },
    ],
  },
  {
    id: 3,
    title: 'Done',
    cards: [
      { id: 4, title: 'Task 4', description: 'Description 4', editing: false },
    ],
  },
]);

const nextColumnId = ref(columns.value.length + 1);
const newColumnName = ref('');
const newTaskTitle = ref<string[]>(Array(columns.value.length).fill(''));

const draggedCard = ref<Card | null>(null);
const draggedFromColumn = ref<Column | null>(null);

const onDragStart = (card: Card, column: Column) => {
  draggedCard.value = card;
  draggedFromColumn.value = column;
};

const onDrop = (targetColumn: Column) => {
  if (draggedCard.value && draggedFromColumn.value) {
    const cardIndex = draggedFromColumn.value.cards.indexOf(draggedCard.value);
    draggedFromColumn.value.cards.splice(cardIndex, 1);
    targetColumn.cards.push(draggedCard.value);
    draggedCard.value = null;
    draggedFromColumn.value = null;
  }
};

const editCard = (card: Card, isEditing: boolean) => {
  card.editing = isEditing;
};

const addColumn = () => {
  if (newColumnName.value.trim() === '') {
    return;
  }
  columns.value.push({
    id: nextColumnId.value++,
    title: newColumnName.value.trim(),
    cards: [],
  });
  newColumnName.value = '';
  newTaskTitle.value.push('');
};

const removeColumn = (index: number) => {
  columns.value.splice(index, 1);
  newTaskTitle.value.splice(index, 1);
};

const addTask = (columnIndex: number, title: string) => {
  if (title.trim() === '') {
    return;
  }
  columns.value[columnIndex].cards.push({
    id: Date.now(),
    title,
    description: '',
    editing: false,
  });
  newTaskTitle.value[columnIndex] = '';
};
</script>

<style scoped>
.custom-background {
  background-color: rgba(242, 184, 180, 1);
  min-height: 100vh;
  padding-top: 20px;
}

.custom-card {
  background-color: rgb(251, 237, 193); 
  border-radius: 16px;
}

</style>