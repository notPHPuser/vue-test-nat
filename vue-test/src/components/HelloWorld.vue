<template>
  <div class="main">
    <div v-show="showImput" class="inputs">
      <input
        class="name_and_phone"
        type="text"
        v-model="name"
        placeholder="Имя"
      />
      <input
        class="name_and_phone"
        type="tel"
        v-mask="'8-###-###-##-##'"
        v-model="phoneNumber"
        placeholder="Номер телефона (через 8)"
      />
      <select class="name_and_phone" v-model="selectedName">
        <option v-for="(name, index) in savedNames" :key="index">
          {{ name }}
        </option>
      </select>

      <button class="button add" @click="saveData">Сохранить</button>
      <!-- <button class="button clear" @click="clearLocalStorage">Отчистить</button> -->
    </div>
    <div class="all_info">
      <button class="show" @click="toggleInput">Добавить контакты</button>
      <table>
        <tr>
          <th>Имя</th>
          <th>Номер телефона</th>
        </tr>
        <tr v-for="(savedData, index) in savedDataList" :key="index">
          <td :class="{ 'new-element': index !== savedDataList.length - 1 }">
            {{ savedDataList[index].name }}
            <span v-if="savedDataList[index].director">
              (директор: {{ savedDataList[index].director }})
            </span>
          </td>
          <td>{{ savedDataList[index].phoneNumber }}</td>
        </tr>
      </table>
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
      showImput: false,
      name: "",
      phoneNumber: "",
      selectedName: "",
      savedDataList: [],
      savedNames: JSON.parse(localStorage.getItem("savedNames")) || [],
    };
  },
  watch: {
    selectedName(newValue) {
      if (newValue !== "") {
        this.showImput = true;
      } else {
        this.showImput = false;
      }
    },
  },
  created() {
    this.savedDataList =
      JSON.parse(localStorage.getItem("savedDataList")) || [];
    this.savedNames = JSON.parse(localStorage.getItem("savedNames")) || [];
    console.log("Сохранные имена:", this.savedNames);
  },

  mounted() {
    this.loadSavedNames();
    this.loadSavedData();
  },
  methods: {
    toggleInput() {
      this.showImput = !this.showImput;
    },
    saveData() {
      if (this.name.trim() !== "" && this.phoneNumber.trim() !== "") {
        const savedData = {
          name: this.name,
          phoneNumber: this.phoneNumber,
          director: this.selectedName,
        };
        const selectedNameIndex = this.savedNames.indexOf(this.selectedName);
        const newIndex =
          selectedNameIndex !== -1
            ? selectedNameIndex + 1
            : this.savedDataList.length;
        this.savedDataList.splice(newIndex, 0, savedData);
        const savedNames = [...this.savedNames];
        savedNames.splice(newIndex, 0, this.name);
        localStorage.setItem("savedNames", JSON.stringify(savedNames));
        console.log("Saved names:", savedNames);
        this.name = "";
        this.phoneNumber = "";
        this.selectedName = "";
        this.loadSavedNames();

        // Сохраняем данные в localStorage
        localStorage.setItem(
          "savedDataList",
          JSON.stringify(this.savedDataList)
        );
        localStorage.setItem("savedNames", JSON.stringify(this.savedNames));
      }
    },
    clearLocalStorage() {
      this.savedDataList = [];
      this.savedNames = [];

      // Очищаем данные из localStorage
      localStorage.removeItem("savedDataList");
      localStorage.removeItem("savedNames");
    },
    loadSavedNames() {
      const savedNames = localStorage.getItem("savedNames");
      if (savedNames) {
        this.savedNames = JSON.parse(savedNames);
      } else {
        this.savedNames = [];
      }
    },
    loadSavedData() {
      const savedDataList = localStorage.getItem("savedDataList");
      if (savedDataList) {
        this.savedDataList = JSON.parse(savedDataList);
      }
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
  position: absolute;
  list-style-type: none;
  padding: 0;

  flex-direction: row;
  height: 20px;
  justify-content: space-between;
}
.all_info {
  position: absolute;
  width: 30%;
  border: 1px solod red;
  top: 100px;
}

.list_item {
  height: 30px;
}

.button {
  position: absolute;
  width: 50%;
  margin: 0 auto;
  top: 30px;
  height: 25px;
}
.add {
  border: none;
  background-color: rgb(109, 255, 109);
  cursor: pointer;
  border-radius: 10px;
  top: 20px;
  position: relative;
}
.inputs {
  position: absolute;
  width: 25%;
  height: 220px;
  justify-content: center;
  left: 50%;
  top: 16.5vh;
  display: flex;
  flex-direction: column;
  /* border: 1px solid red; */
  box-shadow: 0 0 10px 0 grey;
  align-items: center;
  text-align: center;
  border-radius: 5px;
}
/* .name_and_phone:nth-child(1) {
  margin-bottom: 10px;
} */
.name_and_phone {
  width: 60%;
  height: 25px;
  /* border-radius: 10px; */
  border-bottom: 1px solid black;
  border-top: none;
  border-left: none;
  border-right: none;
  margin-bottom: 10px;
  outline: none;
}
table {
  table-layout: fixed;
  width: 100%;
}

td {
  border: 1px solid grey;

  width: 60%;
  height: 40px;
  text-align: center;
  vertical-align: top;
  border-radius: 2px;
  align-items: center;
  font-size: 16px;
}
select {
  text-orientation: vertical;
  line-height: 100%;
}
.clear {
  position: absolute;
  top: -10px;
}
.savedData {
  border: 2px solid red;
  border-radius: 5px;
}
.new-element {
  width: calc(100% - 10px);
}
.show {
  width: 30%;
  height: 30px;
  border-radius: 5px;
  border: none;
  background-color: chartreuse;
  cursor: pointer;
}
</style>
