<script setup lang="ts">
import { ref, defineComponent } from 'vue'
import LabelsInput from '../components/LabelsInput.vue'

const labels = ref([])

function required(value) {
  console.log('validate', value)
  return value != null && value.length > 0 || 'Field is required'
}
</script>

<template>
  <q-page class="flex flex-center">
    <div>
      <LabelsInput v-model="labels" label="Labels input" :rules="[required]" />
    </div>
    <div>
      model value: {{ labels }}
    </div>
  </q-page>
</template>
