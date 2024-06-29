<template>
  <Mentionable :keys="['@', '#']" :items="itemsArray" :insert-space="true">
    <textarea v-model="text" ref="textInput" />

    <template #no-result>
      <div class="dim">No result</div>
    </template>

    <template #item-@="{ item }">
      <div class="user">
        {{ item.value }}
        <span class="dim"> ({{ item.firstName }}) </span>
      </div>
    </template>

    <template #item-#="{ item }">
      <div class="issue">
        <span class="number"> #{{ item.value }} </span>
        <span class="dim">
          {{ item.label }}
        </span>
      </div>
    </template>
    <br />
    <div class="mentioned-text">Mentioned: {{ text }}</div>
  </Mentionable>
</template>

<script>
import { Mentionable } from "vue-mention";

export default {
  components: {
    Mentionable,
  },

  data() {
    return {
      textInput: "",
      itemsArray: [],
      selectedMention: null,
      text: null,
    };
  },

  mounted() {
    this.$refs.textInput.addEventListener("input", (event) => {
      this.text = event.target.value;
    });
  },

  created() {
    this.fetchData();
  },

  methods: {
    handleMention(mention) {
      this.selectedMention = mention;
    },

    async fetchData() {
      try {
        const response = await fetch(
          "https://jsonplaceholder.typicode.com/users"
        );
        const data = await response.json();

        this.itemsArray = data.map((item) => ({
          value: item.name,
          label: item.email,
        }));
      } catch (error) {
        console.error("Error fetching data:", error);
      }
    },
  },
};
</script>

<style>
.mention-item {
  padding: 3px 12px;
  border-radius: 4px;

  /* background-color: blue; */
}

.mention-selected {
  background: rgb(8, 96, 116);
  color: whitesmoke;
}

.mentioned-text {
  display: block;
}

/* Mentionable {
  height: 100%;
} */

input {
  border: none;
  width: 30rem;
  height: 2rem;
  outline: none;
  border-radius: 8px;
  padding: 10px 1rem;
}
</style>
