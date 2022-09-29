<template>
  <div class="container-lg flex justify-center flex-col">
    <div class="block w-1/2 mx-auto mb-4">
      <span class="after:content-['*'] after:ml-0.5 after:text-red-500 block text-sm text-left font-medium text-slate-700">
        Enter text
      </span>
      <textarea
          @focus="textareaFocused"
          ref="textarea"
          @keydown="realKeyboardHandlers"
          @keyup="checkShiftReleased"
          v-model="text"
          class="mt-1 px-3 py-2 bg-white border shadow-sm border-slate-300 placeholder-slate-400 focus:outline-none focus:border-sky-500 focus:ring-sky-500 block w-full rounded-md sm:text-sm focus:ring-1"
          placeholder="Enter text here" />
    </div>
    <div class="block w-1/2 mx-auto mb-4">
      <label for="default-toggle" class="inline-flex relative items-center cursor-pointer">
        <input type="checkbox" v-model="showVirtualKeyboard" id="default-toggle" class="sr-only peer"/>
        <div class="w-11 h-6 bg-gray-200 peer-focus:outline-none peer-focus:ring-4 peer-focus:ring-blue-300 dark:peer-focus:ring-blue-800 rounded-full peer dark:bg-gray-700 peer-checked:after:translate-x-full peer-checked:after:border-white after:content-[''] after:absolute after:top-[2px] after:left-[2px] after:bg-white after:border-gray-300 after:border after:rounded-full after:h-5 after:w-5 after:transition-all dark:border-gray-600 peer-checked:bg-blue-600"></div>
        <span class="ml-3 text-sm font-medium text-gray-900 dark:text-gray-300">Toggle me</span>
      </label>
    </div>
    <div v-show="showVirtualKeyboard" class="bg-slate-300 p-2 w-1/2 mx-auto rounded virtual-keyboard">
      <div class="flex justify-between keyboard-first-row mb-2">
        <button
            v-for="(character, index) in KEYBOARD_CHARACTERS.firstRow"
            @click="() => keyboardClicked(character[getKeyType])"
            :class="`${BUTTON_COMMON_CLASSES} w-10 ${classForRealPressed(character[getKeyType])}`"
            :key="index">
          {{character[getKeyType]}}
        </button>
        <button  @click="removeLastCharacter" :class="`${BUTTON_COMMON_CLASSES} px-2 ${classForRealPressed('Backspace')}`">backspace</button>
      </div>
      <div class="flex justify-between mb-2 keyboard-second-row ">
        <button :class="`${BUTTON_COMMON_CLASSES} px-3 ${classForRealPressed('Tab')}`" @click="() => keyboardClicked(' ')">tab</button>
        <button
            :class="`${BUTTON_COMMON_CLASSES} w-10 ${classForRealPressed(character[getKeyType])}`"
            v-for="(character, index) in KEYBOARD_CHARACTERS.secondRow"
            :ref="character[getKeyType]"
            @click="() => keyboardClicked(character[getKeyType])"
            :key="index">
          {{character[getKeyType]}}
        </button>
      </div>
      <div class="flex justify-between mb-2 keyboard-third-row">
        <button @click="toggleCaps" :class="`${BUTTON_COMMON_CLASSES} px-3 ${isCapsOn ? 'bg-slate-300': ''}`">caps</button>
        <button
            :class="`${BUTTON_COMMON_CLASSES} w-10 ${classForRealPressed(character[getKeyType])}`"
            v-for="(character, index) in KEYBOARD_CHARACTERS.thirdRow"
            :ref="character[getKeyType]"
            @click="() => keyboardClicked(character[getKeyType])"
            :key="index">
          {{character[getKeyType]}}
        </button>
        <button @click="returnPressed" :class="`${BUTTON_COMMON_CLASSES} px-6 ${classForRealPressed('Enter')}`"> enter</button>
      </div>
      <div class="flex justify-between mb-2 keyboard-fourth-row">
        <button @click="toggleShift" :class="`${BUTTON_COMMON_CLASSES} px-5 ${classForRealPressed('Shift')}`">shift</button>
        <button
            :class="`${BUTTON_COMMON_CLASSES} w-10 ${classForRealPressed(character[getKeyType])}`"
            v-for="(character, index) in KEYBOARD_CHARACTERS.fourthRow"
            :ref="character[getKeyType]"
            @click="() => keyboardClicked(character[getKeyType])"
            :key="index">
          {{character[getKeyType]}}
        </button>
        <button @click="toggleShift" :class="`${BUTTON_COMMON_CLASSES} px-5 ${classForRealPressed('Shift')}`">shift</button>
      </div>
      <div class="flex justify-center mb-2 keyboard-fourth-row">
        <button :class="`${BUTTON_COMMON_CLASSES} w-4/6 ${classForRealPressed(' ')}`" @click="() => keyboardClicked(' ')">Space</button>
      </div>
    </div>
  </div>
</template>

<script>
import {KEYBOARD_CHARACTERS} from "@/constant";

export default {
  name: 'Virtual-Keyboard',
  data() {
    return {
      text: '',
      isCapsOn: false,
      isShiftOn: false,
      realKeyPressed: "",
      showVirtualKeyboard: false,
    }
  },

  created() {
    this.KEYBOARD_CHARACTERS = KEYBOARD_CHARACTERS;
    this.BUTTON_COMMON_CLASSES = "bg-slate-50 text-2xl mx-1 h-10 rounded shadow shadow-slate-400 active:bg-slate-300"
  },
  methods: {
    keyboardClicked (character) {
      this.$refs.textarea.focus();
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
    },
    realKeyboardHandlers (e) {
      if (e.key === "CapsLock") {
        this.isCapsOn = !this.isCapsOn
      } else if ( e.key === "Shift") {
        this.isShiftOn = true;
      }
      this.realKeyPressed = e.key;
      if (e.key !== "Shift") {
        setTimeout( () => {
          this.realKeyPressed = "";
        }, 100);
      }
    },
    classForRealPressed (character) {
      return character === this.realKeyPressed ? "bg-slate-300" : "";
    },
    checkShiftReleased(e) {
      if ( e.key === "Shift") {
        this.isShiftOn = false;
        this.realKeyPressed = "";
      }
    },
    textareaFocused() {
      this.showVirtualKeyboard = true;
    },
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
