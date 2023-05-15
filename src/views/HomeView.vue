<template>
  <div class="home">
    <div className="flex justify-center">
      <img alt="Vue logo" src="../assets/logo.png" />
    </div>
    <HelloWorld msg="HelloWorld" />
  </div>
</template>

<script lang="ts">
  import HelloWorld from '@/components/HelloWorld.vue' // @ is an alias to /src
  import { defineComponent } from 'vue'

  export default defineComponent({
    components: {
      HelloWorld,
    },
  })
</script>
