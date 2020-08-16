<template>
  <div class="grid">
    <div class="setting">
      <fieldset>
        <legend>贷款总额（元）</legend>
        <input :value="benjin" @change="handleBenjinChange" type="number"/>
      </fieldset>
      <fieldset>
        <legend>年利率（%）</legend>
        <input :value="lilv" @change="handleLilvChange" type="number"/>
      </fieldset>
      <fieldset>
        <legend>还款期限（月）</legend>
        <input :value="duration" @change="handleDurationChange" type="number"/>
      </fieldset>
    </div>
    <div class="plan-table-wrapper">
      <table class="plan-table">
        <caption>按年还（总利息{{ lixiByYear.toFixed(2) }}，本息合计{{ (lixiByYear + benjin).toFixed(2) }}）</caption>
        <thead>
        <tr>
          <th>还款年</th>
          <th>还款（本息）</th>
        </tr>
        </thead>
        <tbody>
        <tr>
          <td>总利息</td>
          <td>
            {{ lixiByYear.toFixed(2) }}
          </td>
        </tr>
        <tr v-for="(item, i) in Array(Math.ceil(duration/12))">
          <td>第 {{ i + 1 }} 年</td>
          <td>{{ ((benjin - i * (benjin / duration * 12)) * (lilv / 100) + benjin / duration * 12).toFixed(2) }}</td>
        </tr>
        </tbody>
      </table>
      <table class="plan-table">
        <caption>按月还（总利息{{ lixiByMonth.toFixed(2) }}，本息合计{{ (lixiByMonth + benjin).toFixed(2) }}）</caption>
        <thead>
        <tr>
          <th>还款月</th>
          <th>还款（本息）</th>
        </tr>
        </thead>
        <tbody>
        <tr v-for="(item, i) in Array(duration)">
          <td>第 {{ i + 1 }} 个月</td>
          <td>{{ ((benjin - i * (benjin / duration)) * (lilv / 100 / 12) + benjin / duration).toFixed(2) }}</td>
        </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
import { ref } from 'vue';

export default {
  setup() {
    const benjin = ref(0);
    const lilv = ref(4.2);
    const duration = ref(60);

    return {
      benjin,
      lilv,
      duration
    };
  },
  computed: {
    lixiByMonth() {
      return Array(this.duration).fill(1).reduce((prev, current, i) => {
        return (this.benjin - i * (this.benjin / this.duration)) * (this.lilv / 100 / 12) + prev;
      }, 0);
    },
    lixiByYear() {
      return Array(Math.ceil(this.duration / 12)).fill(1).reduce((prev, current, i) => {
        return (this.benjin - i * (this.benjin / this.duration * 12)) * (this.lilv / 100) + prev;
      }, 0);
    }
  },
  methods: {
    handleBenjinChange(evt) {
      this.benjin = +evt.target.value;
    },
    handleLilvChange(evt) {
      this.lilv = +evt.target.value;
    },
    handleDurationChange(evt) {
      this.duration = +evt.target.value;
    }
  }
};
</script>
