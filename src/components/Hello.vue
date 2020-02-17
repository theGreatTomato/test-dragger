<template>
  <div class="fluid container">
    <div class="col-md-3 border-style" v-for="(item, i) in list1" :key="i">
      <draggable class="list-group" tag="ul" :list="item"  :group="{name:'abc',pull:'clone',put:false}" :move="onMoveLeft" @start="isDragging=true" @end="isDragging=false">
        <transition-group type="transition" :name="'flip-list'">
          <li class="list-group-item" v-for="(element, index) in item" :key="index + '1'">
            {{element.name}}
            <span class="badge">{{element.order}}</span>
          </li>
        </transition-group>
      </draggable>
    </div>

    <div class="col-md-3 border-style">
      <draggable element="span" v-model="list2" :group="{name:'abc',pull:'clone',put:true}" :move="onMove" :animation='200'>
        <transition-group name="no" class="list-group" tag="ul">
          <li class="list-group-item" v-for="(element, i) in list2" :key="i">
            {{element.name}}
            <span class="badge">{{element.order}}</span>
          </li>
        </transition-group>
      </draggable>
    </div>

    <div class="list-group col-md-3">
      <pre>{{listString}}</pre>
    </div>
    <div class="list-group col-md-3">
      <pre>{{list2String}}</pre>
    </div>
  </div>
</template>

<script>
import draggable from "vuedraggable";
const message = [
  "vue.draggable",
  "draggable",
  "component",
  "for",
  "vue.js 2.0",
  "based",
  "on",
  "Sortablejs"
];

export default {
  name: "hello",
  components: {
    draggable
  },
  data() {
    return {
      dragOptions: {
          animation: 0,
          group: "description",
          disabled: false,
          ghostClass: "ghost"
      },
      oriList: message.map((name, index) => {
          return { name, order: index + 1, fixed: false };
        }),
      list2: [],
      editable: true,
      isDragging: false,
      delayedDragging: false
    };
  },
  methods: {
    onMoveLeft({ relatedContext, draggedContext }) {
      console.log(this.list1)
    },
    onMove({ relatedContext, draggedContext }) {
      const relatedElement = relatedContext.element;
      const draggedElement = draggedContext.element;
      return (
        (!relatedElement || !relatedElement.fixed) && !draggedElement.fixed
      );
    }
  },
  computed: {
    list1() {
      return message.map((name, index) => {
        return [{ name, order: index + 1, fixed: false }];
      })
    },
    listString() {
      return JSON.stringify(this.list1, null, 2);
    },
    list2String() {
      return JSON.stringify(this.list2, null, 2);
    }
  },
  watch: {
    isDragging(newValue) {
    },
    list1: {
      handler: function () {
        console.log('asd')
      },
      deep: true
    }
  }
};
</script>

<style>
.flip-list-move {
  transition: transform 0.5s;
}

.no-move {
  transition: transform 0s;
}

.ghost {
  opacity: 0.5;
  background: #c8ebfb;
}

.list-group {
  min-height: 20px;
  height: 350px;
}

.list-group-item {
  cursor: move;
}

.list-group-item i {
  cursor: pointer;
}

.border-style {
  border: 1px solid #ddd;
  padding: 5px;
  margin: 5px;
  height: 350px;
}
</style>
