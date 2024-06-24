<template>
    <!-- This component displays a Kanban board with columns and cards -->
    <v-container>
      <v-row>
        <v-col cols="4" v-for="column in columns" :key="column.id">
          <v-card>
            <v-card-title>{{ column.title }}</v-card-title>
              <v-list>
                <v-list-item v-for="card in column.cards" :key="card.id">
                    <v-list-item-title>{{ card.title }}</v-list-item-title>
                    <v-list-item-subtitle>{{ card.description }}</v-list-item-subtitle>
                </v-list-item>
              </v-list>
          </v-card>
        </v-col>
      </v-row>
    </v-container>
  </template>
  
  <script setup lang="ts">
  import { ref } from 'vue';
  
  //structure of card
  interface Card {
    id: number;
    title: string;
    description: string;
  }
  
  //structure of column
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
  </script>
  