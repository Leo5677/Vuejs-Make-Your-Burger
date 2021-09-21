<template>
  <div class="main-container">
    <h1 class="manage-orders">Manage Orders</h1>
    <Dashboard />
  </div>
</template>

<script>
import Dashboard from "../components/Dashboard.vue";

export default {
  name: "Orders",
  components: {
    Dashboard,
  },
};
</script>

<style scoped>
.manage-orders {
  transition-property: color;
  transition-duration: 1s;
  animation: makeburger 2s;
  position: relative;
}

.manage-orders:hover{
  color: #FCBA03;
  cursor: pointer;
}
#manage-orders {
  animation-timing-function: ease;
}

@keyframes makeburger {
  from {
    left: 500px;
  }

  to {
    left: 0px;
  }
}
</style>