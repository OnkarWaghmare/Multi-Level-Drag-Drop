<template>
  <div id="lcontainer" class="container">
    <div
      v-for="item in nestedData.data"
      class="parent"
      :id="item.id"
      draggable="true"
      @dragstart="handleDragStart"
      @dragend="handleDragEnd"
      @dragover="handleDragOver"
      @drop="handleDrop(e, index)"
    >
      {{ item.title }}
      <div
        v-if="item.activities"
        v-for="child in item.activities"
        class="child"
        :id="child.id"
        draggable="true"
        @dragstart="handleDragStart"
        @dragend="handleDragEnd"
        @dragover="handleDragOver"
        @drop="handleDrop(e, index)"
      >
        {{ child.title }}
      </div>
    </div>
  </div>
</template>
<script setup>
import { reactive } from 'vue'

const nestedData = reactive({
  data: [
    {
      id: '1',
      type: 'Resource',
      title: 'video content on how to learn',
      activities: []
    },
    {
      id: '2',
      activities: [
        {
          id: '21',
          type: 'Resource',
          title: 'video content on how to learn'
        },
        {
          id: '31',
          type: 'Resource',
          title: 'video content on how to learn'
        }
      ],
      type: 'process',
      title: 'Learn to learn'
    },
    {
      id: '3',
      activities: [
        {
          id: '31',
          type: 'Resource',
          title: 'How to become a better learner'
        },
        {
          id: '32',
          activities: [
            {
              id: '1',
              activities: [],
              type: 'Resource',
              title: 'Change Your Thought, Change Your Life'
            }
          ],
          type: 'process',
          title: 'how to become a better learner'
        },
        {
          id: '42',
          activities: [
            {
              id: '421',
              type: 'Resource',
              title: 'video content on how to learn'
            },
            {
              id: '422',
              type: 'Resource',
              title: 'video content on how to learn'
            }
          ],
          type: 'process',
          title: 'Learn to learn'
        }
      ],
      type: 'chapter',
      title: 'what is learning'
    }
  ]
})

let draggedId
let targetId

let tempObjNeedToBeRemoved
function iterateNestedData(data, id, action, obj) {
  for (let child of data) {
    if (child.id == id) {
      let index = data.findIndex((ele) => ele.id == id)
      if (action == 'add') {
        data.splice(index, 0, obj)
      } else {
        tempObjNeedToBeRemoved = data.splice(index, 1)[0]
      }
      return
    } else if (child.activities && child.activities.length > 0) {
      iterateNestedData(child.activities, id, action, obj)
    }
  }
}
function handleDragStart(e) {
  draggedId = e.target.id
}
function handleDragOver(e) {
  e.preventDefault()
  targetId = e.target.id
}
function handleDragEnd() {
  iterateNestedData(nestedData.data, draggedId, 'remove', {})
  iterateNestedData(nestedData.data, targetId, 'add', tempObjNeedToBeRemoved)
}
function handleDrop() {
  //
}
</script>
<style>
.container {
  width: 700px;
}
.parent {
  padding: 15px;
  margin: 15px;
  background-color: lightslategray;
}
.child {
  padding: 10px;
  width: 80%;
  margin: 10px;
  background-color: antiquewhite;
}
</style>
