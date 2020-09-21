<template>
  <div id="observer-target"></div>
</template>

<script>
export default {
  name: "VisibilityObserver",
  el: "#observer-target",
  data() {
    return {
      observer: null,
    };
  },
  created() {},
  mounted() {
    this.observer = new IntersectionObserver(this.onElementObserved, {
      root: null,
      threshold: 1.0,
    });

    this.observer.observe(this.$el);
  },
  beforeUnmount() {
    this.observer.disconnect();
  },
  methods: {
    onElementObserved(entries) {
      entries.forEach( en => {
        if (!en.isIntersecting) {
          return;
        }
        this.$emit("became-visible");
      });
    },
  },
};
</script>

<style>
#observer-target {
  width: 1px;
  height: 1px;
}
</style>