<script setup lang="ts">
import { ref } from 'vue'

interface Props {
  modelValue: string[]
  chipSize?: string
  unique?: boolean
}
const props = withDefaults(
  defineProps<Props>(),
  { chipSize: 'md', unique: true }
)

const emit = defineEmits(['update:modelValue'])

const input = ref('')

function removeAtIndex (idx: number): void {
  const newVal = props.modelValue.slice()
  newVal.splice(idx, 1)
  emit('update:modelValue', newVal)
  if (inputEl.value != null) {
    inputEl.value.focus()
  }
}

const inputEl = ref<HTMLInputElement | null>(null)

function onFocused () {
  if (inputEl.value != null) {
    inputEl.value.focus()
  }
}

const field = ref<any>(null)

function addValue () {
  if (input.value.length === 0) {
    return
  }

  if (props.unique && props.modelValue.indexOf(input.value) !== -1) {
    input.value = ''
    return
  }

  const newVal = props.modelValue.slice()
  newVal.push(input.value)
  input.value = ''
  emit('update:modelValue', newVal)
}

function onBackspace () {
  if (input.value.length === 0) {
    const newVal = props.modelValue.slice()
    newVal.pop()
    emit('update:modelValue', newVal)
  }
}

function onValidate (val: unknown) {
  console.log('validate', val)
}
</script>

<template>
  <q-field
    ref="field"
    :stack-label="props.modelValue.length > 0 || input.length > 0"
    class="labels-input"
    @focus="onFocused"
  >
    <template #control>
      <div>
        <q-chip
          v-for="(value, idx) in props.modelValue"
          :key="value"
          removable
          dense
          :size="chipSize ?? 'md'"
          @remove="removeAtIndex(idx)"
        >
          {{ value }}
        </q-chip>
      </div>
      <div class="input">
        <input
          ref="inputEl"
          v-model="input"
          type="text"
          class="q-field__native"
          :placeholder="(($attrs.placeholder ?? '') as string)"
          @keypress.enter.prevent="addValue"
          @keyup.backspace="onBackspace"
        >
      </div>
    </template>
  </q-field>
</template>

<style lang="scss">
.labels-input {
  .input {
    flex-grow: 1;
  }
  input {
    padding: 0 !important;
  }
}
</style>
