<template>
  <v-dialog v-model="isDialogOpen">
    <template #activator="{ on, attrs }">
      <v-btn
        class="mt-4"
        color="primary"
        dark
        v-bind="attrs"
        v-on="on"
      >
        {{ $t('dialog.open') }}
      </v-btn>
    </template>

    <v-card>
      <v-card-title class="text-h5" :class="headerClass">
        {{ header }}
      </v-card-title>

      <v-card-text class="py-8">
        {{ text }}
      </v-card-text>

      <v-card-text v-if="requiresInput" class="pb-8" auto-grow>
        <v-textarea v-model="inputText" />
      </v-card-text>

      <v-divider></v-divider>

      <v-card-actions>
        <v-spacer></v-spacer>
        <v-btn
          text
          class="grey lighten-3"
          @click="isDialogOpen = false"
        >
          {{ $t('dialog.cancel') }}
        </v-btn>
        <v-btn
          color="white"
          text
          class="primary"
          @click="handleOk"
        >
          {{ $t('dialog.ok') }}
        </v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>

<script setup>
import { defineProps, defineEmits, ref, computed, watch } from 'vue'

const props = defineProps({
  header: {
    type: String,
    default: ''
  },
  text: {
    type: String,
    default: ''
  },
  type: {
    type: String,
    default: 'info',
    validator(type) {
      return ['info', 'warning', 'error', 'success'].includes(type)
    }
  },
  requiresInput: {
    type: Boolean,
    default: false
  }
})

const emits = defineEmits(['ok'])

// LAYOUT
const isDialogOpen = ref(false)
const inputText = ref('')

const headerClass = computed(() => {
  const sharedClass = 'white--text'
  let headerClass

  switch (props.type) {
    case 'error':
      headerClass = 'red'
      break

    case 'warning':
      headerClass = 'orange'
      break

    case 'info':
      headerClass = 'light-blue'
      break

    case 'success':
      headerClass = 'light-green'
      break

    default:
      headerClass = 'primary'
      break
  }

  return [sharedClass, headerClass]
})

const handleOk = () => {
  isDialogOpen.value = false
  emits('ok', inputText)
}

watch(isDialogOpen, val => {
  if (val) {
    inputText.value = ''
  }
})
</script>