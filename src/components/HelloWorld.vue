<template>
  <div class="hello">
    <h1 className="text-3xl font-bold text-center">
      COMPUTED, RENDER LIST, PROPS, REACTIVITY
    </h1>

    <p className="text-blue-500">{{ state.count }}</p>
    <button @click="onCreaseCount">increase</button>

    <h1>{{ msg }} -- OR {{ $attrs.class }}</h1>
    <h1 className="text-red-500">{{ title }}</h1>
    <h1 className="text-red-500">
      select: {{ selected }} -- <br />
      -- {{ formatSelect }}
      <br />
      <span className="font-bold">format all: {{ formatAll.selected }}</span>
    </h1>

    <select v-model="selected" @change="onChangeSelect($event)">
      <option
        v-for="(option, index) in listMenu"
        :key="option.name"
        :index="index"
        :value="option.value"
      >
        {{ option.name }}
      </option>
    </select>
    <h1 className="text-3xl font-bold text-center">INPUT NAME</h1>
    <h1>{{ formatFullName }}</h1>
    <h1
      v-if="isValidateFullName"
      :class="{ 'text-blue-500': isValidateFullName }"
    >
      Vue If is awesome!
    </h1>
    <h1 v-show="isValidateFullName">Vue Show is awesome!</h1>
    <!-- <input v-model="fullNameRef" type="text" className="bg-red-300" /> -->
    <input
      ref="inputRef"
      v-model.lazy="fullNameRef"
      type="text"
      className="bg-red-300"
    />

    <br />
    <br />
    <h1 className="text-3xl font-bold text-center">Select filter</h1>
    <div className="shadow-lg p-2 rounded-2xl">
      <div
        v-for="optionValue in selectArr"
        :key="optionValue"
        className="flex justify-center gap-2"
      >
        <div>
          {{ optionValue }}
        </div>
        <button className="bg-red-500" @click="onDeleteSelect(optionValue)">
          close
        </button>
      </div>
    </div>
    <select
      v-if="listDataFilter.length"
      v-model="listDataFilter"
      multiple
      @change="onChangeSelectMultiple($event)"
    >
      <option
        v-for="(option, index) in listDataFilter"
        :key="option.name"
        :index="index"
        :value="option.value"
      >
        {{ option.name }}
      </option>
    </select>

    <h1 className="text-3xl font-bold text-center">PASS COMPONENT</h1>
    <button-component
      first-name="Hoang"
      last-name="Phan"
      @on-click="handleSubmit"
    />
  </div>
</template>

<script lang="ts">
  import { defineComponent, ref, watch } from 'vue'
  import { listMenu } from '@/assets/data/menuDummy'
  import { Ref } from 'vue'
  import { reactive } from 'vue'
  import { watchEffect } from 'vue'
  import ButtonComponent from './ButtonComponent.vue'

  export interface Props {
    msg?: string
  }

  export default defineComponent({
    name: 'HelloWorld',
    components: { ButtonComponent },

    props: {
      msg: {
        required: false,
        type: String,
        default: 'Developer',
      },
    },

    setup() {
      const inputRef = ref<HTMLInputElement | null>(null)
      const fullNameRef = ref('fullName placeholder')
      const state = reactive({ count: 0 })

      const selected: Ref<string> = ref('')
      const selectArr: Ref<string[]> = ref([])

      // onMounted(() => {
      //   inputRef.value?.focus()
      // })

      watchEffect(() => {
        if (inputRef.value) {
          inputRef.value.focus()
        } else {
          // not mounted yet, or the element was unmounted (e.g. by v-if)
        }
      })

      // watch Ref
      watchSelected(selected)
      watchSelectArr(selectArr)

      // watch reactivity
      watch(
        () => state.count,
        (count) => {
          console.log(`count is: ${count}`)
        }
      )

      return { inputRef, fullNameRef, state, selected, selectArr }
    },

    data() {
      return {
        title: 'welcome to my app',
        listMenu,
      }
    },
    computed: {
      formatSelect() {
        const option = listMenu.find((el) => el.value === this.selected)
        return `You choose options: ${option?.name}`
      },
      formatFullName() {
        return `fullName is  ${this.fullNameRef}`
      },
      formatAll() {
        return {
          selected: `You choose options: ${this.selected}`,
          fullNameRef: `fullName is ${this.selected}`,
        }
      },
      isValidateFullName() {
        return !!this.fullNameRef
      },

      listDataFilter() {
        return listMenu.filter((el) => !this.selectArr.includes(el.value))
      },
    },
    methods: {
      onChangeSelect(event: Event) {
        this.selected = (event.target as HTMLInputElement).value
      },
      onChangeSelectMultiple(event: Event) {
        const value = (event.target as HTMLInputElement).value
        this.selectArr = [...this.selectArr, value]
      },
      onDeleteSelect(value: string) {
        this.selectArr = this.selectArr.filter((el) => el !== value)
      },
      onCreaseCount() {
        this.state.count++
      },
      handleSubmit(value: string) {
        console.log('submit', value)
      },
    },
  })

  function watchSelected(selected: any) {
    watch(selected, (newValue, oldValue) => {
      console.log(`selected value changed from ${oldValue} to ${newValue}`)
    })
  }

  function watchSelectArr(selectArr: any) {
    watch(selectArr, (newValue, oldValue) => {
      console.log(`selectArr value changed from ${oldValue} to ${newValue}`)
    })
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
  h3 {
    margin: 40px 0 0;
  }
  ul {
    list-style-type: none;
    padding: 0;
  }
  li {
    display: inline-block;
    margin: 0 10px;
  }
  a {
    color: #42b983;
  }
</style>
