<template>
  <div>
    <SelectTree 
      ref="selectTreeRef"
      :value="form.id"
      multiple
      filterable
      @loadNode="selectTreeNodeLoad"
      @checkChange="onCheckChange"
      @clear="onClear"
    />
  </div>
</template>

<script>
import SelectTree from './selectTree.vue';

export default {
  components: { SelectTree },
  data() {
    return {
      form: {
        id: ''
      }
    }
  },
  methods: {
    async selectTreeNodeLoad(data) {
      let { node, resolve } = data
      if (node.isLeaf) {
        resolve([])
        return 
      }
      // 请求接口返回数据 
    
    },
    onCheckChange(obj) {
      let { data, checked } = obj
      if (!this.form.id) {
        this.form.id = []
      }
      if (checked) {
        this.form.id.push(data.id)
      } else {
        this.form.id = this.form.id.filter(e => e != data.id)
      }
    },
    onClear() {
      this.formData.id = ''
    }
    
  }
}
</script>

<style scoped>

</style>