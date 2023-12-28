<template>
  <div>
    <!-- <Table :fixedColumns="fixedColumns" :scrollableColumns="scrollableColumns" :rows="rows" /> -->
    <trendsFromVue />
  </div>
</template>

<script>
import trendsFromVue from './components/trendsFrom'
export default {
  components: { trendsFromVue },
  data() {
    return {
      fixedColumns: [
        { title: '固定列1', key: 'fixedColumn1', rowspan: 2 },
        { title: '固定列2', key: 'fixedColumn2', rowspan: 2 }
      ],
      scrollableColumns: [
        { title: '滚动列1', key: 'scrollableColumn1' },
        { title: '滚动列2', key: 'scrollableColumn2' },
        { title: '滚动列3', key: 'scrollableColumn3' }
      ],
      rows: [
      { id: 1, fixedColumn1: '固定列1数据', fixedColumn2: '固定列2数据', scrollableColumn1: '滚动列1数据', scrollableColumn2: '滚动列2数据', scrollableColumn3: '滚动列3数据' },
      // 其他数据行
    ],
    };
  }
};
</script>

<style>

</style>
