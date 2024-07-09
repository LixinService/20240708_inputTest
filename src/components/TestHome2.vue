<template>
  <v-container>
    <v-form @submit.prevent="submitForm" :disabled="isSubmitting">
      <!-- 动态显示品项输入框 -->
      <div v-for="(item, index) in items" :key="index" class="item-group">
        <v-select
          v-model="item.value"
          :items="['媒體購買', '廣告投放', '內容行銷']"
          item-text="text"
          item-value="value"
          label="品項"
          variant="outlined"
          :color="colors.primaryColor"
          v-validate="'required'"
        ></v-select>

        <v-text-field
          v-model="item.directions.value"
          :error-messages="item.directions.errorMessage"
          :color="colors.primaryColor"
          label="品項說明"
          variant="outlined"
          v-validate="'max:100'"
        ></v-text-field>

        <!-- 删除按钮 -->
        <v-btn
          :disabled="items.length === 1 && index === 0"
          @click="removeItem(index)"
          >刪除</v-btn
        >
        <!-- 新增按钮 -->
        <v-btn @click="addItem(index + 1)">新增品項</v-btn>
      </div>

      <!-- 確認提交按鈕 -->
      <v-btn type="submit" :disabled="isSubmitting" color="primary">確認</v-btn>
    </v-form>
  </v-container>
</template>

<script setup>
import { reactive } from "vue";
import { useField, useForm } from "vee-validate";
import * as yup from "yup";
import colors from "./config/colors";

const schema = yup.object({
  item: yup.string().required("品項必填"),
  itemDirections: yup.string().max(100, "品項說明長度不符"),
});

const { handleSubmit, isSubmitting } = useForm({
  validationSchema: schema,
});

const items = reactive([
  { value: "", directions: { value: "", errorMessage: null } },
]);

const submitForm = handleSubmit(() => {
  items.forEach((item) => {
    console.log(`品項: ${item.value}, 品項說明: ${item.directions.value}`);
  });
  resetForm();
});

const addItem = (index) => {
  items.splice(index, 0, {
    value: "",
    directions: { value: "", errorMessage: null },
  });
};

const removeItem = (index) => {
  if (items.length > 1) {
    items.splice(index, 1);
  }
};

const resetForm = () => {
  items.forEach((item) => {
    item.value = "";
    item.directions.value = "";
    item.directions.errorMessage = null;
  });
};
</script>

<style scoped>
.item-group {
  display: flex;
  align-items: center;
  gap: 10px;
  margin-bottom: 10px;
}
</style>
