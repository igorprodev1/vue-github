<template>
  <div class="backdrop" v-show="loading">
    <div class="spinner">
      <div class="clip"></div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Loader",

  props: {
    loading: {
      type: Boolean,
      default: true,
    },
  },
};
</script>

<style>
.backdrop {
  background-color: rgba(139, 139, 139, 0.3);
  width: 100%;
  height: 100%;
  position: fixed;
  top: 0px;
  left: 0px;
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 100;
}

.clip {
  height: 35px;
  width: 35px;
  border-width: 3px;
  border-style: solid;
  border-color: #9435f2 transparent;
  border-radius: 100%;
  background: transparent;
}

.spinner {
  text-align: center;
}

.spinner .clip {
  animation: clipDelay 0.75s 0s infinite linear;
  animation-fill-mode: both;

  display: inline-block;
}

@keyframes clipDelay {
  0% {
    -webkit-transform: rotate(0deg);
    transform: rotate(0deg);
  }
  50% {
    -webkit-transform: rotate(180deg);
    transform: rotate(180deg);
  }
  100% {
    -webkit-transform: rotate(360deg);
    transform: rotate(360deg);
  }
}
</style>