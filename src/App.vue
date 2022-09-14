<template>
  <v-app>
    <v-app-bar
      app
      color="primary"
      dark
    >
      <div class="d-flex align-center">
        <v-select
          v-model="$i18n.locale"
          :items="availableLocales"
          filled
          hide-details
          :label="$t('app.language')"
        />
      </div>
    </v-app-bar>

    <v-main>
      <v-container class="d-flex justify-center items-center flex-column">
        <!-- DIALOG 1-->
        <div class="container--inner">
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

          <v-row class="align-center px-2">
            <div class="pr-2">
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

          <transition appear name="transition--fade">
            <div v-if="dialogResponse" class="grey lighten-3 pa-6 my-8 rounded-xl">
              {{ $t('dialog.response') }}: {{ dialogResponse }}
            </div>
          </transition>

          <div class="btn-container">
            <v-btn
              color="primary"
              dark
              @click="$refs.dialogEl1.open()"
            >
              {{ $t('dialog.open') }}
            </v-btn>
          </div>

          <ConfirmationDialog
            ref="dialogEl1"
            v-bind="dialogProps"
            @ok="dialogResponse2 = $event"
          />
        </div>

        <!-- DIALOG 2-->
        <div class="container--inner">
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

          <v-row class="align-center px-2">
            <div class="pr-2">
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

          <transition appear name="transition--fade">
            <div v-if="dialogResponse2" class="grey lighten-3 pa-6 my-8 rounded-xl">
              {{ $t('dialog.response') }}: {{ dialogResponse2 }}
            </div>
          </transition>

          <div class="btn-container">
            <v-btn
              color="primary"
              dark
              @click="$refs.dialogEl2.open()"
            >
              {{ $t('dialog.open') }}
            </v-btn>
          </div>

          <ConfirmationDialog
            ref="dialogEl2"
            v-bind="dialogProps2"
            @ok="handleOk($event, false)"
          />
        </div>
      </v-container>
    </v-main>
  </v-app>
</template>

<script setup>
import { computed, getCurrentInstance, reactive, ref, watch } from 'vue'

// COMPONENTS
import ConfirmationDialog from './components/ConfirmationDialog.vue';

// LANGUAGE
const instance = getCurrentInstance()

const availableLocales = computed(() => instance?.proxy.$i18n.availableLocales)

// LAYOUT
const dialogProps = reactive({
  header: 'Header text 1',
  text: `Lorem ipsum dolor sit amet consectetur adipisicing elit.`,
  requiresInput: false,
  type: 'info'
})

const dialogProps2 = reactive({
  header: 'Header text 2',
  text: `Lorem ipsum dolor sit amet consectetur adipisicing elit.`,
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
const dialogResponse2 = ref('')

const handleOk = (textFromDialog, fromFirstDialog = true) => {
  fromFirstDialog
    ? dialogResponse.value = textFromDialog
    : dialogResponse2.value = textFromDialog
}

watch([dialogResponse, dialogResponse2], () => {
  setTimeout(() => {
    dialogResponse.value = ''
    dialogResponse2.value = ''
  }, 5000)
})
</script>

<style lang="scss" scoped>
.container {
  row-gap: 40px;
  align-items: center;

  &--inner {
    border: 1px solid lightgray;
    padding: 16px;
    border-radius: 16px;
  }
}

.btn-container {
  display: flex;
  justify-content: center;
  margin-top: 24px;
}

.transition--fade-enter-active,
.transition--fade-leave-active {
  transition: all .5s ease;
}

.transition--fade-enter {
  transform: scale(0%);
  opacity: 0;
}

.transition--fade-leave-to {
  transform: translateX(-100%);
  opacity: 0;
}
</style>