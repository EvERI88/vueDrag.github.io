<template>
  <div class="about">
    <form>
      <p>Навзание</p>
      <input type="text" maxlength="30" required v-model="name" />
      <p>Описание</p>
      <textarea required v-model="description" />
      <div class="footer-form">
        <router-link to="/">
          <button class="back" to="/">Отмена</button>
        </router-link>
        <button class="submit" @click.prevent="addCard()">Добавить</button>
      </div>
    </form>
  </div>
</template>

<script>
// @ is an alias to /src
import Cards from "@/components/Cards.vue";
import jsonData from "../assets/jsonData.json";
import { ref } from "vue";

export default {
  name: "AddCards",
  data() {
    return {
      description: "",
      name: "",
      categoryId: 0,
    };
  },
  methods: {
    addCard() {
      let store = JSON.parse(localStorage.getItem("cards"));
      if (store) {
        jsonData = ref(store._rawValue);
      }

      if (this.description == 0 || this.name == 0) {
        return alert("Введите данные");
      }
      const newCard = {
        id: Date.now(),
        name: this.name,
        description: this.description,
        categoryId: 0,
      };
      jsonData._rawValue.push(newCard);

      localStorage.setItem("cards", JSON.stringify(ref(jsonData)));

      this.description = "";
      this.name = "";
    },
  },
  state: {
    name: "",
  },
  components: {
    Cards,
  },
};
</script>
<style scoped>
form {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  margin: 0 auto;
  max-width: 500px;
}

p {
  text-align: left;
  margin-bottom: 0;
  font-size: 18px;
}

a {
  margin-right: 1em;
  width: 50%;
}

.footer-form {
  margin-top: 1em;
  display: flex;
  justify-content: space-between;
}

.back {
  background-color: rgba(197, 32, 32, 0.904);
  color: white;

  width: 100%;
}

.submit {
  background-color: rgb(67, 172, 172);
  color: white;
  width: 50%;
}

button {
  border-radius: 5px;
  height: 40px;
  font-size: 18px;
}

textarea {
  resize: vertical;
}
</style>
