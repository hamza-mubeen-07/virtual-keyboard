<template>
  <div class="container-lg flex justify-center flex-col" >
    <div class="block w-1/2 mx-auto mb-4">
      <span class="after:content-['*'] after:ml-0.5 after:text-red-500 block text-sm text-left font-medium text-slate-700">
        Enter text below
      </span>
      <textarea
          v-model="text"
          class="mt-1 px-3 py-2 bg-white border shadow-sm border-slate-300 placeholder-slate-400 focus:outline-none focus:border-sky-500 focus:ring-sky-500 block w-full rounded-md sm:text-sm focus:ring-1"
          placeholder="you@example.com" />
    </div>
    <div class="bg-slate-300 p-2 w-1/2 mx-auto rounded virtual-keyboard">
      <div class="flex justify-between keyboard-first-row mb-2">
        <button
            :class="`${BUTTON_COMMON_CLASSES} w-10`"
            v-for="(character, index) in KEYBOARD_CHARACTERS.firstRow"
            @click="() => keyboardClicked(character[getKeyType])"
            :key="index">
          {{character[getKeyType]}}
        </button>
        <button @click="removeLastCharacter" :class="`${BUTTON_COMMON_CLASSES} px-2`">backspace</button>
      </div>
      <div class="flex justify-between mb-2 keyboard-second-row ">
        <button :class="`${BUTTON_COMMON_CLASSES} px-3`" @click="() => keyboardClicked(' ')">tab</button>
        <button :class="`${BUTTON_COMMON_CLASSES} w-10`" v-for="(character, index) in KEYBOARD_CHARACTERS.secondRow" @click="() => keyboardClicked(character[getKeyType])" :key="index">
          {{character[getKeyType]}}
        </button>
      </div>
      <div class="flex justify-between mb-2 keyboard-third-row">
        <button @click="toggleCaps" :class="`${BUTTON_COMMON_CLASSES} px-3 ${isCapsOn ? 'bg-slate-300': ''}`">caps</button>
        <button :class="`${BUTTON_COMMON_CLASSES} w-10`" v-for="(character, index) in KEYBOARD_CHARACTERS.thirdRow" @click="() => keyboardClicked(character[getKeyType])" :key="index">
          {{character[getKeyType]}}
        </button>
        <button @click="returnPressed" :class="`${BUTTON_COMMON_CLASSES} px-6`"> enter</button>
      </div>
      <div class="flex justify-between mb-2 keyboard-fourth-row">
        <button @click="toggleShift" :class="`${BUTTON_COMMON_CLASSES} px-5`">shift</button>
        <button :class="`${BUTTON_COMMON_CLASSES} w-10`" v-for="(character, index) in KEYBOARD_CHARACTERS.fourthRow" @click="() => keyboardClicked(character[getKeyType])" :key="index">
          {{character[getKeyType]}}
        </button>
        <button @click="toggleShift" :class="`${BUTTON_COMMON_CLASSES} px-5`">shift</button>
      </div>
      <div class="flex justify-center mb-2 keyboard-fourth-row">
        <button :class="`${BUTTON_COMMON_CLASSES} w-4/6`" @click="() => keyboardClicked(' ')">Space</button>
      </div>
    </div>
  </div>
</template>

<script>
import {KEYBOARD_CHARACTERS} from "@/constant"
export default {
  name: 'Virtual-Keyboard',
  data() {
    return {
      text: 'Hello World!',
      isCapsOn: false,
      isShiftOn: false,
    }
  },
  created() {
    this.KEYBOARD_CHARACTERS = KEYBOARD_CHARACTERS;
    this.BUTTON_COMMON_CLASSES = "bg-slate-50 text-2xl mx-1 h-10 rounded shadow shadow-slate-400 active:bg-slate-300"
  },
  methods: {
    keyboardClicked (character) {
      this.text = this.text + character;
    },
    removeLastCharacter () {
      this.text  = this.text.slice(0, -1)
    },
    toggleCaps() {
      this.isCapsOn = !this.isCapsOn;
    },
    toggleShift() {
      this.isShiftOn = !this.isShiftOn;
    },
    returnPressed() {
      this.text += '\n';
    }
  },
  computed: {
    getKeyType () {
      return this.isShiftOn ?
          "onShift"
          : this.isCapsOn ?
               "onCaps"
              : "noCaps"
    }
  }
}
</script>

<style>

</style>
