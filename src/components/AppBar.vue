<template>
  <v-app-bar app>
    <v-toolbar-title>導覽列示例</v-toolbar-title>
    <v-spacer></v-spacer>
    <!-- 專案管理 -->
    <v-menu v-model="projectManagementMenu" offset-y>
      <template v-slot:activator="{ on, attrs }">
        <v-btn
          v-bind="attrs"
          v-on="on"
          @click="toggleMenu('projectManagementMenu')"
          :style="{ color: projectManagementMenu ? '#FF6421' : 'white' }"
          text
        >
          專案管理
          <v-icon
            right
            :style="{ color: projectManagementMenu ? '#FF6421' : 'white' }"
            >mdi-menu-down</v-icon
          >
        </v-btn>
      </template>
      <v-list>
        <v-list-item-group>
          <v-list-item>
            <v-list-item-content
              @mouseover="hoverMenu('projectIncomeMenu', true)"
              @mouseleave="hoverMenu('projectIncomeMenu', false)"
            >
              專案收入
              <v-menu
                v-model="projectIncomeMenu"
                absolute
                offset-y
                :style="{ left: '100%', top: 0 }"
              >
                <template v-slot:activator="{ on, attrs }">
                  <v-icon v-bind="attrs" v-on="on" right>mdi-menu-right</v-icon>
                </template>
                <v-list>
                  <v-list-item>
                    <v-list-item-content>進單</v-list-item-content>
                  </v-list-item>
                  <v-list-item>
                    <v-list-item-content>收入請款</v-list-item-content>
                  </v-list-item>
                  <v-list-item>
                    <v-list-item-content>開發票</v-list-item-content>
                  </v-list-item>
                  <v-list-item>
                    <v-list-item-content>收款</v-list-item-content>
                  </v-list-item>
                </v-list>
              </v-menu>
            </v-list-item-content>
          </v-list-item>
          <v-list-item>
            <v-list-item-content>專案執行</v-list-item-content>
          </v-list-item>
          <v-list-item>
            <v-list-item-content>專案成本</v-list-item-content>
          </v-list-item>
        </v-list-item-group>
      </v-list>
    </v-menu>
    <!-- 營業費用 -->
    <v-menu v-model="businessExpensesMenu" offset-y>
      <template v-slot:activator="{ on, attrs }">
        <v-btn
          v-bind="attrs"
          v-on="on"
          @click="toggleMenu('businessExpensesMenu')"
          :style="{ color: businessExpensesMenu ? '#FF6421' : 'white' }"
          text
        >
          營業費用
          <v-icon
            right
            :style="{ color: businessExpensesMenu ? '#FF6421' : 'white' }"
            >mdi-menu-down</v-icon
          >
        </v-btn>
      </template>
      <v-list>
        <v-list-item>
          <v-list-item-content>營業費用項目1</v-list-item-content>
        </v-list-item>
        <v-list-item>
          <v-list-item-content>營業費用項目2</v-list-item-content>
        </v-list-item>
      </v-list>
    </v-menu>
    <!-- 財務管理 -->
    <v-menu v-model="financialManagementMenu" offset-y>
      <template v-slot:activator="{ on, attrs }">
        <v-btn
          v-bind="attrs"
          v-on="on"
          @click="toggleMenu('financialManagementMenu')"
          :style="{ color: financialManagementMenu ? '#FF6421' : 'white' }"
          text
        >
          財務管理
          <v-icon
            right
            :style="{ color: financialManagementMenu ? '#FF6421' : 'white' }"
            >mdi-menu-down</v-icon
          >
        </v-btn>
      </template>
      <v-list>
        <v-list-item>
          <v-list-item-content>財務管理項目1</v-list-item-content>
        </v-list-item>
        <v-list-item>
          <v-list-item-content>財務管理項目2</v-list-item-content>
        </v-list-item>
      </v-list>
    </v-menu>
  </v-app-bar>
</template>

<script setup>
import { ref } from "vue";

const projectManagementMenu = ref(false);
const projectIncomeMenu = ref(false);
const businessExpensesMenu = ref(false);
const financialManagementMenu = ref(false);

const toggleMenu = (menu) => {
  if (menu === "projectManagementMenu") {
    projectManagementMenu.value = !projectManagementMenu.value;
  } else if (menu === "projectIncomeMenu") {
    projectIncomeMenu.value = !projectIncomeMenu.value;
  } else if (menu === "businessExpensesMenu") {
    businessExpensesMenu.value = !businessExpensesMenu.value;
  } else if (menu === "financialManagementMenu") {
    financialManagementMenu.value = !financialManagementMenu.value;
  }
};

const hoverMenu = (menu, isHovered) => {
  if (menu === "projectIncomeMenu") {
    projectIncomeMenu.value = isHovered;
  }
};
</script>

<style scoped>
.v-list-item-content {
  cursor: pointer;
}
</style>
