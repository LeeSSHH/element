<template>
  <table @click="handleQuarterTableClick" class="el-quarter-table">
    <tbody>
      <tr>
        <td :class="getCellStyle(0)">
          <a class="cell">{{ t("el.datepicker.quarter1") }}</a>
        </td>
        <td :class="getCellStyle(1)">
          <a class="cell">{{ t("el.datepicker.quarter2") }}</a>
        </td>
      </tr>
      <tr>
        <td :class="getCellStyle(2)">
          <a class="cell">{{ t("el.datepicker.quarter3") }}</a>
        </td>
        <td :class="getCellStyle(3)">
          <a class="cell">{{ t("el.datepicker.quarter4") }}</a>
        </td>
      </tr>
    </tbody>
  </table>
</template>

<script type="text/babel">
import Locale from "element-ui/src/mixins/locale";
import { isDate, range, firstMonthOfQuarter, quarterOfMonth } from "../util";
import { hasClass } from "element-ui/src/utils/dom";

const monthsInQuarter = (quarter) => {
  const firstMonth = firstMonthOfQuarter(quarter);
  return range(3).map((n) => firstMonth + n);
};
export default {
  props: {
    disabledQuarter: {},
    value: {},
    defaultValue: {
      validator(val) {
        // null or valid Date Object
        return val === null || (val instanceof Date && isDate(val));
      },
    },
    date: {},
  },
  mixins: [Locale],
  methods: {
    getCellStyle(quarter) {
      const style = {};
      const year = this.date.getFullYear();

      style.disabled =
        typeof this.disabledQuarter === "function"
          ? monthsInQuarter(quarter).every(this.disabledQuarter)
          : false;
      style.current =
        this.value.getFullYear() === year &&
        quarterOfMonth(this.value.getMonth()) === quarter;
      console.log(quarterOfMonth(this.value.getMonth()), quarter);
      return style;
    },

    handleQuarterTableClick(event) {
      const target = event.target;
      if (target.tagName !== "A") return;
      if (hasClass(target.parentNode, "disabled")) return;
      const column = target.parentNode.cellIndex;
      const row = target.parentNode.parentNode.rowIndex;
      const quarter = row * 2 + column;

      this.$emit("pick", quarter);
    },
  },
};
</script>
