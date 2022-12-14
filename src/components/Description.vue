<template>
  <div class="description">
    <div class="description-container">
      <h3 class="description-title">{{ this.cardName }}</h3>
      <div class="description-content">
        <div class="description-text">
          <p v-on:click="toggleDescription" class="description-subtitle">
            Description <span>Edit <img src="/images/edit.png" /></span>
          </p>
          <p v-if="descriptionText">{{ this.descriptionText }}</p>
          <p v-else>no description</p>
          <textarea v-if="showDescription" v-model="descriptionText"></textarea>
        </div>
        <div v-if="showDescription" class="description-controls">
          <Button
            value="Edit"
            appearance="primary"
            @click="addDescription(this.cardId)"
          />
        </div>

        <Checklists :cardId="this.cardId" />

        <Comments :cardId="this.cardId" />
      </div>
      <Button
        value="Cancel"
        appearance="danger"
        @click.prevent="$emit('closeDescription')"
      />
    </div>
  </div>
</template>

<script>
import Button from "./Button.vue";
import Checklists from "./Checklists.vue";
import Comments from "./Comments.vue";
export default {
  components: { Button, Comments, Checklists },
  data() {
    return {
      showModal: false,
      showDescription: false,
      descriptionText: "",
      apiKey: "dc599fe2c56f5a3c881cc6c67bbd95af",
      apiToken:
        "6a8b79d7762879acb352ad2b3f0715fd4f53e1710aa6ef9cbdaee0adeb1de3e5",
    };
  },
  created() {
    this.fetchDescriptionText(this.cardId);
  },
  props: {
    cardName: {
      type: String,
      default: "",
    },
    boardId: {
      type: Number,
    },
    cardId: {
      type: Number,
    },
  },
  methods: {
    async fetchDescriptionText(cardId) {
      const response = await fetch(
        `https://api.trello.com/1/cards/${cardId}?key=${this.apiKey}&token=${this.apiToken}`,
        {
          method: "GET",
          headers: {
            Accept: "application/json",
          },
        }
      );
      const card = await response.json();
      this.descriptionText = card.desc;
    },
    async addDescription(cardId) {
      await fetch(
        `https://api.trello.com/1/cards/${cardId}?key=${this.apiKey}&token=${this.apiToken}&desc=${this.descriptionText}`,
        {
          method: "PUT",
          headers: {
            Accept: "application/json",
          },
        }
      ).then((response) => {
        this.fetchDescriptionText(cardId);
        console.log(`Response: ${response.status} ${response.statusText}`);
        return response.text();
      });
    },
    toggleDescription() {
      this.showDescription = !this.showDescription;
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
  &-content {
    text-align: left;
    font-size: 20px;
    font-weight: bold;
  }
  &-text {
    p:nth-child(2) {
      font-size: 15px;
      padding-left: 20px;
    }
    textarea {
      width: 100%;
      height: 100px;
      display: block;
    }
  }
  &-subtitle {
    width: fit-content;
    &:hover {
      color: rgb(173, 27, 27);
    }
    &:hover span {
      display: inline;
    }
    span {
      display: none;
      color: black;
    }
    img {
      width: 15px;
    }
  }
  &-controls {
    padding-top: 20px;
    width: 25%;
    margin: 0 auto;
    text-align: center;
  }
  &-checklist {
    &__title:hover {
      color: rgb(173, 27, 27);
    }
    &__content {
      border: #555151 2px solid;
      border-radius: 10px;
    }
    &__name {
      padding-left: 20px;
      text-decoration: underline;
    }
    &__item {
      width: 100%;
      font-size: 15px;
    }
  }
  &-comments {
    &__title:hover {
      color: rgb(173, 27, 27);
    }
    &__content {
      border: #555151 2px solid;
      border-radius: 10px;
      padding-left: 20px;
    }
  }
}
</style>
