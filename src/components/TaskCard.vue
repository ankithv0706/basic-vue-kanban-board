<template>
  <div :class="['card', colorClass]">
    <h1>{{ card.title }}</h1>
  </div>
</template>

<script>
export default {
  props: ["card"],
  computed: {
    colorClass: function () {
      switch (this.card.color) {
        case "blue":
          return "bg-primary";
        case "green":
          return "bg-success";
        case "light blue":
          return "bg-info";
        case "yellow":
          return "bg-warning";
        case "red":
          return "bg-danger";
        default:
          break;
      }
    },
  },
};
</script>
