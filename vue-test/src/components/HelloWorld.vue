<template>
  <div class="main">
    <div class="inputs">
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
    </div>
    <div class="all_info">
      <ul class="list">
        <li
          class="list_item"
          v-for="(savedData, index) in savedDataList"
          :key="index"
        >
          <span class="span_li">{{ savedData.name }}</span>
          <span class="span_li">{{ savedData.phoneNumber }}</span>
        </li>
      </ul>
    </div>
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

<style>
.main {
  width: 70%;
  margin: 0 auto;
  /* border: 1px solid red; */
}
.list {
  position: relative;
  list-style-type: none;
  padding: 0;

  flex-direction: row;
  height: 20px;
  justify-content: space-between;
}
.all_info {
  position: relative;
  width: 20%;
  border: 1px solod red;
}
.inputs {
  position: relative;
  width: 20%;
  height: 100px;
  justify-content: space-between;
  left: 30%;
  display: flex;
  flex-direction: column;
}
.list_item {
  display: inline-block;
  margin-right: 10px;
}
.span_li:nth-child(1) {
  border-right: 1px solid black;
}
</style>
