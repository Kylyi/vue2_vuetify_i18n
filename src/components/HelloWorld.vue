<template>
  <v-container class="d-flex justify-center items-center flex-column">
    <!-- FORM - DIALOG 1-->
    <v-text-field
      v-model="dialogProps.header"
      :label="$t('dialog.headerText')"
    />
    <v-text-field
      v-model="dialogProps.text"
      :label="$t('dialog.text')"
    />
    <v-switch
      v-model="dialogProps.requiresInput"
      :label="$t('dialog.requiresInput')"
      class="mt-0 mb-1"
    />

    <v-row class="align-center">
      <div class="px-3">
        {{ $t('dialog.type') }}:
      </div>

      <v-btn-toggle :value="dialogProps.type">
        <v-btn
          v-for="(dialogType, idx) in dialogTypes"
          :key="idx"
          :class="[
            isTypeToggled(dialogType.value, dialogProps)
              ? `${dialogType.color} white--text`
              : `${dialogType.color}--text`
          ]"
          @click="dialogProps.type = dialogType.value"
        >
          {{ $t(dialogType.label) }}
        </v-btn>
      </v-btn-toggle>
    </v-row>

    <div v-if="dialogResponse" class="white--text green pa-6 my-8 rounded-xl">
      {{ $t('dialog.response') }}: {{ dialogResponse }}
    </div>

    <!-- DIALOG -->
    <ConfirmationDialog v-bind="dialogProps" @ok="handleOk" />


    <hr class="my-12">

    <!-- FORM - DIALOG 2-->
    <v-text-field
      v-model="dialogProps2.header"
      :label="$t('dialog.headerText')"
    />
    <v-text-field
      v-model="dialogProps2.text"
      :label="$t('dialog.text')"
    />
    <v-switch
      v-model="dialogProps2.requiresInput"
      :label="$t('dialog.requiresInput')"
      class="mt-0 mb-1"
    />

    <v-row class="align-center">
      <div class="px-3">
        {{ $t('dialog.type') }}:
      </div>


      <v-btn-toggle :value="dialogProps2.type">
        <v-btn
          v-for="(dialogType, idx) in dialogTypes"
          :key="idx"
          :class="[
            isTypeToggled(dialogType.value, dialogProps2)
              ? `${dialogType.color} white--text`
              : `${dialogType.color}--text`
          ]"
          @click="dialogProps2.type = dialogType.value"
        >
          {{ $t(dialogType.label) }}
        </v-btn>
      </v-btn-toggle>
    </v-row>

    <div v-if="dialogResponse" class="white--text green pa-6 my-8 rounded-xl">
      {{ $t('dialog.response') }}: {{ dialogResponse }}
    </div>

    <!-- DIALOG -->
    <ConfirmationDialog v-bind="dialogProps2" @ok="handleOk" />
  </v-container>
</template>

<script setup>
import { reactive, ref } from 'vue'

// COMPONENTS
import ConfirmationDialog from './ConfirmationDialog';

// LAYOUT
const dialogProps = reactive({
  header: 'Header text 1',
  text: `Lorem ipsum dolor sit amet consectetur adipisicing elit. Cumque
    blanditiis sint aperiam accusantium, unde mollitia incidunt inventore
    tempore officia vitae repellendus nisi numquam accusamus ipsa, labore
    consectetur nostrum quo dignissimos?`,
  requiresInput: false,
  type: 'info'
})

const dialogProps2 = reactive({
  header: 'Header text 1',
  text: `Lorem ipsum dolor sit amet consectetur adipisicing elit. Cumque
    blanditiis sint aperiam accusantium, unde mollitia incidunt inventore
    tempore officia vitae repellendus nisi numquam accusamus ipsa, labore
    consectetur nostrum quo dignissimos?`,
  requiresInput: true,
  type: 'error'
})

const dialogTypes = [
  { label: 'state.error', value: 'error', color: 'red' },
  { label: 'state.warning', value: 'warning', color: 'orange' },
  { label: 'state.info', value: 'info', color: 'light-blue' },
  { label: 'state.success', value: 'success', color: 'light-green' },
]

const isTypeToggled = (type, dialogProps) => dialogProps.type === type

// ACTIONS
const dialogResponse = ref('')

const handleOk = (textFromDialog) => {
  dialogResponse.value = textFromDialog
}
</script>