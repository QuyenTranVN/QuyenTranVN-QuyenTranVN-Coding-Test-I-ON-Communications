<!-- @format -->

<template>
  <div>
    <div class="top pt-5">
      <v-row align="center" justify="center">
        <v-btn depressed @click="onSave"> Save </v-btn>
        <v-btn depressed> Undo </v-btn>
        <v-btn depressed> Redo </v-btn>
        <v-btn depressed> Export </v-btn>
        <v-btn depressed> Import </v-btn>
        <v-btn depressed>
          <nuxt-link to="/" target="_blank"> View </nuxt-link>
        </v-btn>
      </v-row>
    </div>
    <v-divider />

    <div class="middle d-flex">
      <div class="side_bar">
        <section>
          <div v-for="(drag, index) in dragComponent" @dragstart="moveElement($event)" id="draggable" :key="index" v-draggable.dragComponent="drag">
            <img src="https://placehold.co/60" alt="" draggable="false" />
            <p id="dragtarget">{{ drag }}</p>
          </div>
        </section>
      </div>
      <v-divider vertical></v-divider>

      <div id="el" class="content" @mousemove="getPosition($event)">
        <div class="menu">
          <ul>
            <li>mouse:{{ x }}, {{ y }}</li>
            <li>dragging:{{ dragging }}</li>
            <li>instances: {{ formArrayTotal.length }}</li>
            <li>
              config:
              <span v-if="textProp.type === 'button'"> {'id': "{{ textProp.id }}", 'component' : {{ textProp.type }}, 'props': {'text': {{ textProp.buttonTextForm }}, 'message': {{ textProp.buttonAlertForm }}} </span>
              <span v-if="textProp.type === 'paragraph'"> {'id': "{{ textProp.id }}",'component' : {{ textProp.type }}, 'props': {'text' : {{ textProp.paragraphForm }}) </span>
            </li>
          </ul>
        </div>
        <section id="drop" v-droppable.dragComponent="send">
          <div v-for="(item, index) in formArrayTotal" :key="index + item">
            <span v-if="item.type === 'button'">
              <v-btn @click="inputButton(index)">{{ item.buttonTextForm != '' ? item.buttonTextForm : 'BUTTON' }}</v-btn>
            </span>
            <span v-else @click="insertParagraph(index)">
              <p>
                {{ item.paragraphForm != '' ? item.paragraphForm : 'paragraph' }}
              </p>
            </span>
          </div>
        </section>
      </div>
    </div>
    <v-divider />
    <div v-for="(inputForm, index) in formArrayTotal" :key="index" class="bottom pt-5">
      <div v-if="indexButton === index && inputForm.type === 'paragraph'">
        <v-form>
          <label for="">Paragraph Text </label>
          <v-text-field @keyup="textPara(inputForm)" v-model="inputForm.paragraphForm" outlined />
        </v-form>
      </div>
      <div v-if="indexButton === index && inputForm.type === 'button'">
        <v-form action="">
          <label for="">Button text </label>
          <v-text-field @keyup="textButton(inputForm)" v-model="inputForm.buttonTextForm" outlined />

          <label for="">Alert message </label>
          <v-text-field @keyup="textAlert(inputForm)" v-model="inputForm.buttonAlertForm" outlined />
        </v-form>
      </div>
    </div>
  </div>
</template>

<script>
import { defineComponent, ref } from '@nuxtjs/composition-api'
import axios from 'axios'

export default defineComponent({
  setup() {
    const x = ref(0)
    const y = ref(0)
    const inputText = ref(false)
    const inputTextButton = ref(false)
    const indexButton = ref(-1)
    const formArrayTotal = ref([])
    const uuidv4 = require('uuid/v4')
    const dragging = ref('')
    const getElement = () => {
      formArrayTotal.value.forEach((item) => {
        return item
      })
    }

    const insertParagraph = (index) => {
      indexButton.value = index
      inputText.value = !inputText.value
    }

    const inputButton = (index) => {
      indexButton.value = index
      inputTextButton.value = !inputTextButton.value
    }

    const getPosition = (event) => {
      x.value = event.pageX
      y.value = event.pageY
      return { x, y }
    }

    const dragComponent = ref(['Paragraph', 'Button'])
    const getDrags = ref([])

    const send = ($ev) => {
      if ($ev === '"Button"') {
        formArrayTotal.value.push({
          id: uuidv4(),
          type: 'button',
          buttonTextForm: '',
          buttonAlertForm: '',
        })
      } else {
        formArrayTotal.value.push({
          id: uuidv4(),
          type: 'paragraph',
          paragraphForm: '',
        })
      }
      getElement()
    }

    const paragraphForm = ref('')
    const buttonTextForm = ref('')
    const buttonAlertForm = ref('')

    const textProp = ref('')

    const textPara = (text) => {
      textProp.value = text
    }
    const textButton = (text) => {
      textProp.value = text
    }
    const textAlert = (text) => {
      textProp.value = text
    }

    const moveElement = (event) => {
      dragging.value = event.target.innerText
    }

    const onSave = () => {
      formArrayTotal.value.forEach((item) => {
        if (item.buttonTextForm === '') {
          item.buttonTextForm = 'button'
        }
        if (item.paragraphForm === '') {
          item.paragraphForm = 'paragraph'
        }
        axios.post('http://localhost:3000/posts', item)
      })
    }

    return { moveElement, textButton, textAlert, textProp, textPara, onSave, dragging, formArrayTotal, indexButton, paragraphForm, buttonTextForm, buttonAlertForm, inputTextButton, inputText, inputButton, insertParagraph, send, getPosition, x, y, dragComponent, getDrags }
  },
})
</script>

<style lang="scss">
#drop {
  width: 100%;
  height: 100%;
  display: flex;
  flex-flow: column;
}
.top {
  height: 80px;
}
.middle {
  height: calc(100vh - 200px);
  .content {
    width: 80%;
    height: calc(100vh - 200px);
    align-items: flex-start;
    flex-direction: row;
    .menu {
      width: 50%;
      ul {
        list-style: none;
      }
    }
  }
  .side_bar {
    width: 200px;
    border-right: 1px solid black;
    padding-right: 40px;
  }
}
</style>
