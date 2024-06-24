<template>
    <!-- This component displays a Kanban board with columns and cards -->
    <v-container>
      <v-row>
        <v-col cols="4" v-for="column in columns" :key="column.id">
          <v-card>
            <v-card-title>{{ column.title }}</v-card-title>
            <v-card-text>
              <v-list>
                <v-list-item v-for="card in column.cards" :key="card.id">
                  <v-list-item-content>
                    <v-list-item-title>{{ card.title }}</v-list-item-title>
                    <v-list-item-subtitle>{{ card.description }}</v-list-item-subtitle>
                  </v-list-item-content>
                </v-list-item>
              </v-list>
              <v-text-field v-model="newCardTitle" label="New Card Title" outlined dense></v-text-field>
              <v-textarea v-model="newCardDescription" label="New Card Description" outlined dense></v-textarea>
              <v-btn class="custom-button rounded-button" @click="addCard(column.id)">Add Card</v-btn>
            </v-card-text>
          </v-card>
        </v-col>
      </v-row>
    </v-container>
  </template>
  
  <script setup lang="ts">
  import { ref } from 'vue';
  
  interface Card {
    id: number;
    title: string;
    description: string;
  }
  
  interface Column {
    id: number;
    title: string;
    cards: Card[];
  }
  
  const columns = ref<Column[]>([
    { id: 1, title: 'To Do', cards: [{ id: 1, title: 'Task 1', description: 'Description 1' }] },
    { id: 2, title: 'Doing', cards: [] },
    { id: 3, title: 'Done', cards: [] }
  ]);
  
  const newCardTitle = ref('');
  const newCardDescription = ref('');
  
  function addCard(columnId: number) {
    const column = columns.value.find(col => col.id === columnId);
    if (column) {
      column.cards.push({
        id: Date.now(),
        title: newCardTitle.value.trim(),
        description: newCardDescription.value.trim()
      });
      newCardTitle.value = '';
      newCardDescription.value = '';
    }
  }
  </script>
  