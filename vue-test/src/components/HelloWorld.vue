<template>
  <div>
    <input
      class="name_and_phone"
      type="text"
      v-model="name"
      placeholder="Имя"
    />
    <input
      class="name_and_phone"
      type="tel"
      v-mask="'#-###-###-##-##'"
      v-model="phoneNumber"
      placeholder="Номер телефона (через 8)"
    />
    <button class="add_button" @click="saveData">Сохранить</button>
    <button class="clear_button" @click="clearLocalStorage">Отчистить</button>
    <ul class="list">
      <li
        class="list_item"
        v-for="(savedData, index) in savedDataList"
        :key="index"
      >
        {{ savedData.name }} - {{ savedData.phoneNumber }}
      </li>
    </ul>
  </div>
</template>

<script>
import { mask } from "vue-the-mask";

export default {
  directives: {
    mask,
  },
  data() {
    return {
      name: "",
      phoneNumber: "",
      savedDataList: [],
    };
  },
  mounted() {
    this.savedDataList =
      JSON.parse(localStorage.getItem("savedDataList")) || [];
  },
  methods: {
    saveData() {
      if (this.name.trim() !== "" && this.phoneNumber.trim() !== "") {
        const savedData = {
          name: this.name,
          phoneNumber: this.phoneNumber,
        };
        this.savedDataList.push(savedData);
        localStorage.setItem(
          "savedDataList",
          JSON.stringify(this.savedDataList)
        );
        this.name = "";
        this.phoneNumber = "";
      }
    },
    clearLocalStorage() {
      localStorage.clear();
      this.savedDataList = [];
    },
  },
};
</script>
