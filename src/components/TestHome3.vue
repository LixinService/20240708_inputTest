<template>
  <v-container>
    <v-form @submit.prevent="submitForm">
      <v-row>
        <v-col cols="6">
          <v-select
            v-model="formData.item"
            label="品項"
            outlined
            :items="['媒體購買', '廣告投放', '內容行銷']"
            :color="colors.primaryColor"
            required
          ></v-select>
        </v-col>
        <v-col cols="6">
          <v-text-field
            v-model="formData.itemDirections"
            label="品項說明"
            outlined
            :color="colors.primaryColor"
            required
          ></v-text-field>
        </v-col>
      </v-row>

      <v-row>
        <v-col>
          <v-btn type="submit" color="primary">確認</v-btn>
        </v-col>
      </v-row>
    </v-form>

    <v-divider class="my-4"></v-divider>
  </v-container>

  <v-container>
    <v-data-table
      :headers="tableHeaders"
      :items="tableData"
      :items-per-page="5"
      class="elevation-1"
    >
      <template v-slot:item.item="{ item }">
        <span>{{ item.item }}</span>
      </template>
      <template v-slot:item.itemDirections="{ item }">
        <span>{{ item.itemDirections }}</span>
      </template>
      <template v-slot:item.actions="{ item }">
        <v-icon @click="editItem(item)">mdi-pencil</v-icon>
        <v-icon @click="deleteItem(item)">mdi-delete</v-icon>
      </template>
    </v-data-table>
  </v-container>
</template>

<script setup>
import { ref, reactive } from "vue";
import colors from "./config/colors";

const formData = reactive({
  item: "",
  itemDirections: "",
});

const tableData = ref([]);
const tableHeaders = [
  { title: "品項", value: "item" },
  { title: "品項說明", value: "itemDirections" },
  { title: "操作", value: "actions" },
];

const submitForm = () => {
  if (editIndex === null) {
    // 新增模式
    tableData.value.push({
      item: formData.item,
      itemDirections: formData.itemDirections,
    });
  } else {
    // 編輯模式
    tableData.value[editIndex] = {
      item: formData.item,
      itemDirections: formData.itemDirections,
    };
    editIndex = null; // 重置編輯索引
  }
};

let editIndex = null;

const editItem = (item) => {
  formData.item = item.item;
  formData.itemDirections = item.itemDirections;
  editIndex = tableData.value.indexOf(item);
};

const deleteItem = (item) => {
  const index = tableData.value.indexOf(item);
  if (index !== -1) {
    tableData.value.splice(index, 1);
  }
};

const resetForm = () => {
  formData.item = "";
  formData.itemDirections = "";
};
</script>

<style scoped>
.v-data-table__actions {
  text-align: right;
}
</style>
