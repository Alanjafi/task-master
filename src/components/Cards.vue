<template>
  <v-list-item
    :draggable="true"
    @dragstart="onDragStart"
    @dragover.prevent
    @drop="onDrop"
  >
    <template v-if="!card.editing">
      <v-list-item-content @click="editCard(true)">
        <v-list-item-title>{{ card.title }}</v-list-item-title>
        <v-list-item-subtitle>{{ card.description }}</v-list-item-subtitle>
      </v-list-item-content>
    </template>
    <template v-else>
      <v-list-item-content>
        <v-text-field v-model="card.title" label="Title" outlined dense></v-text-field>
        <v-textarea v-model="card.description" label="Description" outlined dense></v-textarea>
        <v-btn @click="saveEdit">Save</v-btn>
        <v-btn @click="editCard(false)">Cancel</v-btn>
      </v-list-item-content>
    </template>
  </v-list-item>
</template>

<script setup lang="ts">
const props = defineProps({
  card: Object,
  column: Object
});
const emit = defineEmits(['drag-start', 'drop', 'edit-card']);

const onDragStart = () => {
  emit('drag-start', props.card, props.column);
};

const onDrop = () => {
  emit('drop', props.column);
};

const editCard = (isEditing: boolean) => {
  emit('edit-card', props.card, isEditing);
};

const saveEdit = () => {
  editCard(false);
};
</script>
