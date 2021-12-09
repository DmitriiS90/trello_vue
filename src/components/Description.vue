<template>
  <div class="description">
    <div class="description-container">
      <h3 class="description-title">{{ this.name }}</h3>
      <div class="description-text">
        <p>Description</p>
        <textarea></textarea>
      </div>
      <div class="description-controls">
        <Button value="Save" appearance="primary" />
        <Button
          value="Cancel"
          appearance="danger"
          @click="$emit('closeDescription')"
        />
      </div>
    </div>
  </div>
</template>

<script>
import Button from "./Button.vue";
export default {
  components: { Button },
  data() {
    return {
      showModal: false,
    };
  },
  props: {
    name: {
      type: String,
      default: "",
    },
  },
};
</script>

<style lang="scss" scoped>
.description {
  position: fixed;
  z-index: 1;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(131, 129, 129, 0.5);
  display: table;
  &-title {
    margin: 0 auto;
    width: 70%;
    padding: 10px;
    border-bottom: rgba(95, 92, 92, 0.5) 2px solid;
  }
  &-container {
    width: 30%;
    margin: 10% auto;
    padding: 20px 30px;
    background-color: #fff;
    border-radius: 2px;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.33);
    font-family: Helvetica, Arial, sans-serif;
  }
  &-text {
    p {
      text-align: left;
      font-size: 20px;
      font-weight: bold;
    }
    textarea {
      width: 100%;
      height: 100px;
    }
  }
  &-controls {
    padding-top: 20px;
    width: 25%;
    margin: 0 auto;
    display: flex;
    align-items: center;
    justify-content: space-between;
  }
}
</style>