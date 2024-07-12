<template>
    <q-btn type="reset" class="q-pa-xs" @click="handleClick" flat size="md" :ripple="false" no-caps style="text-decoration: underline;">Clear All</q-btn>
</template>

<script setup>

const emit = defineEmits(['click']);

const handleClick = (event) => {
  emit('click', event);
};
 
</script>