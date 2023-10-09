<template>
  <div class="block">
    <div class="move" style="align-items: center">
      <label class="toggler-wrapper style-1">
        <input type="checkbox" v-model="checked" @click="toCheck" />
        <div class="toggler-slider">
          <div class="toggler-knob"></div>
        </div>
      </label>
      <p style="padding-left:15px;">Запретить перемещение блоков</p>
    </div>
    <div class="body">
      <div class="category" v-for="category in categories" key="category.id">
        <h3>{{ category.name }}</h3>
        <div
          @create="createCard"
          class="droppable blockCard"
          @drop="onDrop($event, category.id)"
          @dragover.prevent
          @dragenter.prevent
        >
          <div
            class="card draggble"
            v-for="card in items.filter((x) => x.categoryId === category.id)"
            key="card.id"
            @dragstart="onDragStart($event, card)"
            draggable="true"
          >
            <h2>{{ card.name }}</h2>
            <h3>{{ card.description }}</h3>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import jsonData from "../assets/jsonData.json";
import { ref } from "vue";

export default {
  name: "Cards",
  data() {
    return {
      checked: false,
      jsonData: jsonData,
      local: "",
    };
  },

  methods: {
    toCheck() {
      const items = document.getElementsByClassName("draggble");
      for (let i = 0; i < items.length; i++) {
        if (!this.checked) {
          items[i].removeAttribute("draggable");
        } else {
          items[i].setAttribute("draggable", true);
        }
      }
    },
    getItems: function () {
      this.local = ref(JSON.parse(localStorage.getItem("cards")));
    },
  },
  setup() {
    const setLocal = JSON.parse(localStorage.getItem("cards"));
    let items = ref(jsonData);
    if (setLocal) {
      items = ref(setLocal._rawValue);
    }

    const categories = ref([
      {
        id: 0,
        name: "1 Столбец",
      },
      {
        id: 1,
        name: "2 Столбец",
      },
      {
        id: 2,
        name: "3 Столбец",
      },
      {
        id: 3,
        name: "4 Столбец",
      },
    ]);
    function onDragStart(e, card) {
      e.dataTransfer.dropEffect = "move";
      e.dataTransfer.effectAllowed = "move";
      e.dataTransfer.setData("itemId", card.id.toString());
    }

    function onDrop(e, categoryId) {
      const itemId = parseInt(e.dataTransfer.getData("itemId"));
      items.value = items.value.map((x) => {
        if (x.id == itemId) x.categoryId = categoryId;
        localStorage.setItem("cards", JSON.stringify(items));
        return x;
      });
    }
    return {
      items,
      categories,
      onDragStart,
      onDrop,
      setLocal,
    };
  },
  beforeMount() {
    this.getItems();
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.block {
  display: flex;
  justify-content: space-between;
  flex-direction: column;
}

.body {
  display: flex;
  flex-direction: row;
  justify-content: center;
  margin: 0 auto;
}

.category {
  display: flex;
  flex-direction: column;
  max-width: 1200px;
  width: 100%;
  align-items: center;
  margin: 0px 10px;
}

.blockCard {
  border: 2px solid;
  border-color: rgb(6, 153, 153);
  background-color: rgba(0, 0, 0, 0);
  border-radius: 5px;
  min-height: 50px;
  height: 100%;
  width: 170px;
}

.card {
  border: 1px solid;
  border-color: rgb(8, 209, 209);
  background-color: white;
  margin: 5px;
  border-radius: 5px;
}
.move {
  align-items: center;
  position: absolute;
  top: 0;
  display: flex;
  
}
h2,
h3 {
  margin: 0;
  word-wrap: break-word;
}

@charset "UTF-8";
.toggler-wrapper {
  display: block;
  width: 45px;
  height: 25px;
  cursor: pointer;
  position: relative;
}

.toggler-wrapper input[type="checkbox"] {
  display: none;
}

.toggler-wrapper input[type="checkbox"]:checked+.toggler-slider {
  background-color: rgb(6, 153, 153);
}

.toggler-wrapper .toggler-slider {
  background-color: #ccc;
  position: absolute;
  border-radius: 100px;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  -webkit-transition: all 300ms ease;
  transition: all 300ms ease;
}

.toggler-wrapper .toggler-knob {
  position: absolute;
  -webkit-transition: all 300ms ease;
  transition: all 300ms ease;
}

.toggler-wrapper.style-1 input[type="checkbox"]:checked+.toggler-slider .toggler-knob {
  left: calc(100% - 19px - 3px);
}

.toggler-wrapper.style-1 .toggler-knob {
  width: calc(25px - 6px);
  height: calc(25px - 6px);
  border-radius: 50%;
  left: 3px;
  top: 3px;
  background-color: #fff;
}
</style>
