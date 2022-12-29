<template>
  <div>
    <input v-model="name"/>
    <button @click="getText">Suche</button>
  </div>
  <br/><br/>
  <div>
    <ul>
      <li v-for="result in results" :key="result.id">
        <div v-if="editing !== result.id">
          <div class="result-item">
            <img class="result-thumb" :src="result.thumb" alt="image">
            <div class="result-description" v-html="highlight(result.description)"></div>
            <button @click="editing = result.id">edit {{ result.id }}</button>
          </div>
        </div>
        <div v-else>
          <div class="result-item">
            <img class="result-thumb" :src="result.thumb" alt="image">
            <textarea class="description top-left" v-model="result.description"/>
            <button @click="updateText(result)">Save</button>
            <button @click="editing = null">Cancel</button>
          </div>
        </div>
      </li>
    </ul>
  </div>

</template>

<style scoped>
.result-item {
  display: flex;
  align-items: center;
}

.description {
  width: 600px;
  height: 300px;
}

.result-thumb {
  width: 600px;
  height: 300px;
  object-fit: cover;
}

.result-description {
  margin-left: 100px;

}

.result-id {
  margin-left: 100px;
}

button {
  font-weight: bold;
  background: aquamarine;
}

</style>
<script>
import {api_patch, api_search, replaceAll} from "@/requests.js";

export default {
  data() {
    return {
      editing: null,
      results: [],
      name: "",
    };
  },
  methods: {
    async getText() {
      this.results = await api_search(this.name);
    },
    async updateText(result) {
      await api_patch(result);
    },
    highlight(text) {
      return replaceAll(text, this.name, `${this.name.bold()}`)
    }
  }
};
</script>
