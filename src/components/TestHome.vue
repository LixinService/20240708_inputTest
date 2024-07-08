<template>
  <v-container>
    <v-form @submit.prevent="submit" :disabled="isSubmitting">
      <!-- 品項 -->
      <v-select
        v-model="item.value.value"
        :items="itemItems.items"
        item-title="text"
        item-value="value"
        label="品項"
        variant="outlined"
        :color="colors.primaryColor"
      ></v-select>

      <!-- 品項說明 -->
      <v-text-field
        label="品項說明"
        v-model="itemDirections.value.value"
        :error-messages="itemDirections.errorMessage.value"
        :color="colors.primaryColor"
        variant="outlined"
      ></v-text-field>

      <!-- 起始日 -->
      <v-menu
        v-model="menuStart"
        :close-on-content-click="false"
        :nudge-right="40"
        :color="colors.primaryColor"
        variant="outlined"
        transition="scale-transition"
      >
        <template v-slot:activator="{ attrs }">
          <v-text-field
            :model-value="selectedDateStartText"
            :error-messages="startDate.errorMessage.value"
            variant="outlined"
            label="起始日"
            :append-icon="menuStart ? 'mdi-close' : 'mdi-calendar'"
            readonly
            v-bind="attrs"
            @click="menuStart = !menuStart"
          ></v-text-field>
        </template>

        <v-date-picker
          v-model="startDate.value.value"
          no-title
          @input="menuStart = false"
        >
          <template v-slot:actions>
            <v-btn @click="menuStart = false">確定</v-btn>
          </template>
        </v-date-picker>
      </v-menu>

      <!-- 結束日 -->
      <v-menu
        v-model="menuEnd"
        :close-on-content-click="false"
        :nudge-right="40"
        :color="colors.primaryColor"
        variant="outlined"
        transition="scale-transition"
      >
        <template v-slot:activator="{ attrs }">
          <v-text-field
            :model-value="selectedDateEndText"
            :error-messages="endDate.errorMessage.value"
            variant="outlined"
            label="結束日"
            :append-icon="menuEnd ? 'mdi-close' : 'mdi-calendar'"
            readonly
            v-bind="attrs"
            @click="menuEnd = !menuEnd"
          ></v-text-field>
        </template>
        <v-date-picker
          v-model="selectedDateEnd"
          no-title
          @input="menuEnd = false"
        >
          <template v-slot:actions>
            <v-btn @click="menuEnd = false">確定</v-btn>
          </template>
        </v-date-picker>
      </v-menu>

      <!-- 數量 -->
      <v-text-field
        label="數量"
        v-model="number.value.value"
        :error-messages="number.errorMessage.value"
        :color="colors.primaryColor"
        variant="outlined"
      >
      </v-text-field>

      <!-- 單位 -->
      <!-- <v-text-field
            label="單位"
            v-model="unit.value"
            readonly
            :error-messages="unit.errorMessage.value"
            :color="colors.primaryColor"
            variant="outlined"
          >
          </v-text-field> -->

      <!-- 報價金額(未) -->
      <v-text-field
        label="報價金額(未)"
        v-model="quoteNotTaxed.value.value"
        :error-messages="quoteNotTaxed.errorMessage.value"
        :color="colors.primaryColor"
        variant="outlined"
      >
      </v-text-field>

      <!-- 報表金額(未) -->
      <v-text-field
        label="報表金額(未)"
        v-model="reportNotTaxed.value.value"
        :error-messages="reportNotTaxed.errorMessage.value"
        :color="colors.primaryColor"
        variant="outlined"
      >
      </v-text-field>

      <!-- 是否續約 -->
      <!-- <v-select
            label="是否續約"
            v-model="renewContract.value.value"
            :items="renewContractItems.items"
            item-text="text"
            item-value="value"
            :color="colors.primaryColor"
            variant="outlined"
          >
          </v-select> -->

      <v-btn type="submit" :disabled="isSubmitting"> 確認 </v-btn>
    </v-form>

    <v-data-table :headers="tableHeaders" :items="tableData">
      <template v-slot:items="props">
        <td>{{ props.item.item }}</td>
      </template>
    </v-data-table>
  </v-container>
</template>

<script setup>
import { computed, ref, reactive, watch } from "vue";
import { useField, useForm } from "vee-validate";
import validator from "validator";
import * as yup from "yup";
import colors from "./components/config/colors";

// 品項
const itemItems = reactive({
  selected: null,
  items: [
    { text: "廣告投放", value: "廣告投放" },
    { text: "媒體購買", value: "媒體購買" },
    { text: "內容行銷", value: "內容行銷" },
  ],
});

// 是否續約
const renewContractItems = reactive({
  selected: null,
  items: [
    { text: "是", value: true },
    { text: "否", value: false },
  ],
});
// 起日期
const menuStart = ref(false);
const selectedDateStart = ref(null);

// 起日期_轉換日期格式
const selectedDateStartText = computed(() => {
  startDate.value.value
    ? new Date(startDate.value.value).toLocaleDateString().substring(0, 10)
    : "";
  // if (!selectedDateStart.value) return "";

  // const dateObj = new Date(startDate.value.value);
  // const year = dateObj.getFullYear();
  // const month = String(dateObj.getMonth() + 1).padStart(2, "0"); // 补零操作
  // const date = String(dateObj.getDate()).padStart(2, "0"); // 补零操作

  // return `${year}/${month}/${date}`;
});

// 結束期
const menuEnd = ref(false);
const selectedDateEnd = ref(null);

// 結束期_轉換日期格式
const selectedDateEndText = computed(() => {
  if (!selectedDateEnd.value) return "";

  const dateObj = new Date(selectedDateEnd.value);
  const year = dateObj.getFullYear();
  const month = String(dateObj.getMonth() + 1).padStart(2, "0"); // 补零操作
  const date = String(dateObj.getDate()).padStart(2, "0"); // 补零操作

  return `${year}/${month}/${date}`;
});

// 表格欄位
const tableData = reactive([]);

const tableHeaders = [
  { text: "品項" },
  { text: "品項說明" },
  { text: "起始日" },
  { text: "結束日" },
  { text: "數量" },
  // { text: "單位" },
  // { text: "單位金額" },
  { text: "報價金額(未)" },
  { text: "報表金額(未)" },
  // { text: "是否續約" },
  // { text: "續約提醒日" },
  // { text: "續約金額(未)" },
];

const schema = yup.object({
  // 品項
  item: yup.string().required("品項必填"),
  // 品項說明
  itemDirections: yup.string().max(100, "品項說明長度不符"),
  // 起始日
  startDate: yup.string().required("起始日必填"),
  // 結束日
  endDate: yup.string().required("結束日必填"),
  // 數量
  number: yup.number().required("數量必填"),
  // 單位
  // unit: yup.string(),
  // 單位金額
  // unitMoney: yup.number(),
  // 報價金額(未)
  quoteNotTaxed: yup.number().required("報價金額(未)必填"),
  // 報表金額(未)
  reportNotTaxed: yup.number().required("報表金額(未)必填"),
  // 是否續約
  // renewContract: yup.boolean().required("是否續約必填"),
  // 續約提醒日
  // renewalReminderDate: yup.string().when("renewContract", {
  //   is: true,
  //   then: yup.string().required("續約提醒日必填"),
  // }),
  //續約金額(未)
  // renewalReminderMoneyNotTaxed: yup.number().when("renewContract", {
  //   is: true,
  //   then: yup.string().required("續約提醒日必填"),
  // }),
});

const { handleSubmit, isSubmitting } = useForm({
  validationSchema: schema,
});

const item = useField("item");
const itemDirections = useField("itemDirections");
const startDate = useField("startDate");
const endDate = useField("endDate");
const number = useField("number");
const unit = useField("unit");
const unitMoney = useField("unitMoney");
const quoteNotTaxed = useField("quoteNotTaxed");
const reportNotTaxed = useField("reportNotTaxed");
const renewContract = useField("renewContract");
const renewalReminderDate = useField("renewalReminderDate");
const renewalReminderMoneyNotTaxed = useField("renewalReminderMoneyNotTaxed");

const submit = handleSubmit((values) => {
  tableData.push(values);
});

watch(
  () => itemItems.selected,
  (newValue) => {
    if (newValue === "廣告投放") {
      unit.value.value = "個";
    } else if (newValue === "媒體購買") {
      unit.value.value = "天";
    } else {
      unit.value.value = "項";
    }
  }
);
</script>
