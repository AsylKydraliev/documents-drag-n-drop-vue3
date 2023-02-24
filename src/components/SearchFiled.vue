<template>
  <div>
    <q-input
        v-model="value"
        label="Поиск"
        class="search-field"
        clearable
        @update:model-value="$emit('search', value)"
    >
      <template v-slot:prepend>
        <q-icon name="search" />
      </template>
    </q-input>
  </div>
</template>

<script>
import { ref } from 'vue';
export default {
  name: 'SearchField',
  setup() {
    const value = ref('');

    return {
      value,
    }
  },
}
</script>
<style scoped>
.search-field {
  width: 564px;
}
</style>