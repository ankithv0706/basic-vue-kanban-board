<template>
  <div class="modal-mask">
    <div class="modal-wrapper">
      <div class="modal-container">
        <div class="modal-header">
          <slot name="header"
            ><input type="text" v-model="selectedCard.title" />
          </slot>
        </div>

        <div class="modal-body">
          <slot name="body">
            <textarea
              v-model="selectedCard.description"
              placeholder="description"
            />
          </slot>
          <br />
          <select v-model="selectedCard.color">
            <option value="blue">blue</option>
            <option value="green">green</option>
            <option value="light blue">light blue</option>
            <option value="yellow">yellow</option>
            <option value="red">red</option>
          </select>
        </div>

        <div class="modal-footer">
          <slot name="footer">
            <button class="modal-default-button" @click="$emit('close')">
              Close
            </button>
            <button
              class="modal-default-button"
              @click="$emit('save', selectedCard)"
            >
              Save
            </button>
          </slot>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  props: ["card"],
  data() {
    return {
      selectedCard: { ...this.card },
    };
  },
  methods: {
    save() {
      this.$emit("save", this.selectedCard);
    },
  },
};
</script>