<template>
  <div>
    <div id="form">
      <form id="burger-form" @submit="postBurger">
        <div class="input-container">
          <label for="name">Client Name</label>
          <input
            type="text"
            id="name"
            name="name"
            v-model="name"
            placeholder="Type your name..."
          />
        </div>
        <div class="input-container">
          <label for="bread">Choose Your Bread:</label>
          <select class="select" name="bread" id="bread" v-model="bread">
            <option value="">Choose Your Bread...</option>
            <option
              class="select"
              v-for="bread in breads"
              :key="bread.id"
              :value="bread.type"
            >
              {{ bread.type }}
            </option>
          </select>
        </div>
        <div class="input-container">
          <label for="meat">Choose Your Meat:</label>
          <select class="select" name="meat" id="meat" v-model="meat">
            <option value="">Choose Your Meat...</option>
            <option v-for="meat in meats" :key="meat.id" :value="meat.type">
              {{ meat.type }}
            </option>
          </select>
        </div>
        <div class="input-container" id="optionals-container">
          <label for="optionals" id="optionals-title">Choose Optionals:</label>
          <div
            class="checkbox-container"
            v-for="optional in optionalsdata"
            :key="optional.id"
          >
            <input
              type="checkbox"
              name="optionals"
              v-model="optionals"
              :value="optional.type"
            />
            <span>{{ optional.type }}</span>
          </div>
        </div>
        <div class="input-container">
          <input type="submit" class="submit-btn" value="Create My Burger" />
        </div>
        <br>
        <Message :msg="msg" v-show="msg" />
      </form>
    </div>
  </div>
</template>

<script>
import Message from "./Message.vue";

export default {
  name: "BurgerForm",

  data() {
    return {
      breads: null,
      meats: null,
      optionalsdata: null,
      name: null,

      bread: null,
      meat: null,
      optionals: [],
      msg: null,
    };
  },
  methods: {
    async getIngredients() {
      const req = await fetch("http://localhost:3000/ingredients");
      const data = await req.json();

      this.breads = data.breads;
      this.meats = data.meats;
      this.optionalsdata = data.optionals;
    },
    async postBurger(e) {
      e.preventDefault();
      const data = {
        name: this.name,
        meat: this.meat,
        bread: this.bread,
        optionals: Array.from(this.optionals),
        status: "Requested",
      };
      const dataJson = JSON.stringify(data);
      const req = await fetch("http://localhost:3000/burgers", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: dataJson,
      });
      const res = await req.json();

      this.msg = `Requested Successfuly - Nº ${res.id}`;
      setTimeout(() => (this.msg = ""), 3000);

      this.name = "";
      this.bread = "";
      this.meat = "";
      this.optionals = "";
    },
  },
  mounted() {
    this.getIngredients();
  },
  components: {
    Message,
  },
};
</script>

<style scoped>
#burger-form {
  border: 2px solid #222;
  max-width: 400px;
  margin: 0 auto;
  padding: 50px;
  transition-property: background-color border-radius border-color;
  transition-duration: 1s;
}

#burger-form:hover {
  background-color: rgb(255, 253, 245);
  border-radius: 6px;
  border: 2px solid #fcba03;
}

.input-container {
  display: flex;
  flex-direction: column;
  margin-bottom: 20px;
}

label {
  font-weight: bold;
  margin-bottom: 15px;
  color: #222;
  padding: 5px 10px;
  border-left: 4px solid #fcba03;
}

input,
select {
  padding: 5px 10px;
  width: 300px;
}

.select {
  cursor: pointer;
}

#optionals-container {
  flex-direction: row;
  flex-wrap: wrap;
}

#optionals-title {
  width: 100%;
}

.checkbox-container {
  display: flex;
  align-items: flex-start;
  width: 50%;
  margin-bottom: 20px;
}

.checkbox-container span,
.checkbox-container input {
  width: auto;
  cursor: pointer;
}

.checkbox-container span {
  margin-left: 6px;
  font-weight: bold;
}

.submit-btn {
  background-color: #222;
  color: #fcba03;
  font-weight: bold;
  border: 2px solid #222;
  border-radius: 5px;
  padding: 10px;
  font-size: 16px;
  margin: 0 auto;
  cursor: pointer;
  transition: 0.5s;
}

.submit-btn:hover {
  background-color: transparent;
  color: #222;
}
</style>