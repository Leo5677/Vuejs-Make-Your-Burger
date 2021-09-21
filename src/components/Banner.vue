<template>
  <div id="main-banner">
    <h1 class="h1-banner">Make Your Burger</h1>
  </div>
</template>

<script>
export default {
  name: "Banner",
};
</script>

<style scoped>
#main-banner {
  background-image: url("/img/burger.jpg");
  background-position: 0 -250px;
  background-size: cover;
  height: 500px;
  display: flex;
  align-items: center;
  justify-content: flex-start;
}

#main-banner h1 {
  color: #fff;
  text-align: center;
  font-size: 60px;
  padding: 20px 40px;
  transition: .5s;
  color: #fcba03;
  cursor: pointer;
}

.h1-banner {
  animation-name: h1banner;
  animation-duration: 2s;
  animation-fill-mode: forwards;
  position: relative;
}

#h1-banner {
  animation-timing-function: ease;
}

@keyframes h1banner {
  from {
    top: -200px;
  }

  to {
    top: 0px;
  }
}

#main-banner h1:hover {
  color: #fff;
}
</style>