<template>
  <button class="button button-close" @click="onClick">
    Hello {{ firstName }} {{ lastName }}
    <slot></slot>
  </button>

  <!-- <button class="button button-close" @click="$emit('onClick', 'child')">
    Hello {{ firstName }} {{ lastName }}
  </button> -->
</template>

<script setup lang="ts">
  import { defineEmits, defineProps } from 'vue'

  const emits = defineEmits(['onClick'])

  const onClick = () => {
    emits('onClick', 'child value')
  }

  defineProps({
    firstName: {
      type: String,
      required: true,
    },
    lastName: {
      type: String,
      required: true,
    },
  })
</script>

<style scoped>
  .button {
    border: none;
    border-radius: 2px;
  }

  .button-close {
    background-color: red;
  }
</style>
