<template>
  <div class="burger-table" id="burger-table" v-if="burgers">
    <Message :msg="msg" v-show="msg" />
    <div>
      <div id="burger-table-heading">
        <div class="order-id" style="color: #FCBA03">#</div>
        <div style="color: #a77a00">Client</div>
        <div style="color: #a77a00">Bread</div>
        <div style="color: #a77a00">Meat</div>
        <div style="color: #a77a00">Optionals</div>
        <div style="color: #a77a00">Actions</div>
      </div>
    </div>
    <div id="burger-table-rows">
      <div class="burger-table-row" v-for="burger in burgers" :key="burger.id">
        <div class="order-number">{{ burger.id }}</div>
        <div>{{ burger.name }}</div>
        <div>{{ burger.bread }}</div>
        <div>{{ burger.meat }}</div>
        <div>
          <ul v-if="burger.optionals">
            <li v-for="optional in burger.optionals" :key="optional.id">
              {{ optional }}
            </li>
          </ul>
          <ul>
            <li>None</li>
          </ul>
        </div>
        <div>
          <select
            name="status"
            class="status"
            @change="updateBurger($event, burger.id)"
          >
            <option value="">Select</option>
            <option
              v-for="s in status"
              :key="s.id"
              :value="s.type"
              :selected="burger.status == s.type"
            >
              {{ s.type }}
            </option>
          </select>
          <button class="delete-btn" @click="deleteBurger(burger.id)">
            Cancel
          </button>
        </div>
      </div>
    </div>
  </div>
  <div v-else>
    <h1 style="margin-top: 150px; font-size: 2rem">No Order...</h1>
  </div>
</template>

<script>
import Message from "./Message.vue";

export default {
  name: "Dashboard",
  data() {
    return {
      burgers: null,
      burger_id: null,
      status: [],
      msg: null,
    };
  },
  methods: {
    async getOrders() {
      const req = await fetch("http://localhost:3000/burgers");
      const data = await req.json();

      this.burgers = data;

      this.getStatus();
    },
    async getStatus() {
      const req = await fetch("http://localhost:3000/status");
      const data = await req.json();

      this.status = data;
    },

    async deleteBurger(id) {
      const req = await fetch(`http://localhost:3000/burgers/${id}`, {
        method: "DELETE",
      });

      const res = await req.json();

      this.getOrders();

      this.msg = `Order Nº ${id} - Canceled`;
      setTimeout(() => (this.msg = ""), 3000);
    },

    async updateBurger(event, id) {
      const option = event.target.value;

      const dataJson = JSON.stringify({ status: option });

      const req = await fetch(`http://localhost:3000/burgers/${id}`, {
        method: "PATCH",
        headers: { "Content-Type": "application/json" },
        body: dataJson,
      });

      const res = await req.json();

      this.msg = `Order Nº ${res.id} - Updated for: ${res.status}`;
      setTimeout(() => (this.msg = ""), 3000);
    },
  },
  mounted() {
    this.getOrders();
  },
  components: {
    Message,
    Message,
  },
};
</script>

<style scoped>
#burger-table {
  max-width: 1200px;
  margin: 0 auto;
}

.burger-table {
  animation: burgertable;
  animation-duration: 2s;
  animation-fill-mode: forwards;
  position: relative;
}

#burger-table {
  animation-timing-function: ease;
}

@keyframes burgertable {
  from {
    bottom: -100px;
  }
  to {
    bottom: 0px;
  }
}

#burger-table-heading,
#burger-table-rows,
.burger-table-row {
  display: flex;
  flex-wrap: wrap;
}

#burger-table-heading {
  font-weight: bold;
  padding: 12px;
  border-bottom: 3px solid #333;
}

.burger-table-row {
  width: 100%;
  padding: 12px;
  border-bottom: 1px solid #ccc;
}

#burger-table-heading div,
.burger-table-row div {
  width: 19%;
}

.burger-table-row {
  border: 3px solid transparent;
  border-bottom: 3px solid rgb(226, 226, 226);
}

.burger-table-row:hover {
  transition: 0.5s;
  border: 3px solid #fcba03;
  border-radius: 5px;
  background-color: rgb(255, 253, 245);
  cursor: pointer;
}

#burger-table-heading .order-id,
.burger-table-row .order-number {
  width: 5%;
}

select {
  padding: 12px 6px;
  margin-right: 12px;
}

.delete-btn {
  background-color: #222;
  color: #fcba03;
  font-weight: bold;
  border: 2px solid #222;
  padding: 10px;
  font-size: 16px;
  margin: 0 auto;
  cursor: pointer;
  transition: 0.5s;
}

.status:hover {
  cursor: pointer;
}

.delete-btn:hover {
  background-color: transparent;
  color: #222;
}
</style>