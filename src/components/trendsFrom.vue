<template>
  <div class="pg_view">
    <el-form ref="formRef" :model="formData">
      <el-form-item
        v-for="(item, i) in formListShow"
        :key="i"
        :label="item.label"
        :prop="item.prop"
      >
        <el-input 
          v-if="item.type === 'input'"
          v-model.trim="formData[item.prop]"
          :placeholder="item.placeholder"
          clearable
        />
        <el-select
          v-else-if="item.type == 'select'"
          v-model="formData[item.prop]"
          :placeholder="item.placeholder"
          clearable
        >
          <el-option 
            v-for="item2 in item.options"
            :key="item2.value"
            :label="item2.label"
            :value="item2.value"
          />
        </el-select>
      </el-form-item>
    </el-form>

    <el-row>
      <el-popover
        ref="addItemPop"
        placement="bottom"
        trigger="click"
        width="180"
        popper-class="addItem"
        @show="popShow"
      >
        <el-checkbox-group
          v-model="items"
        >
          <el-checkbox 
            v-for="item in formList"
            :key="item.value"
            :label="item.label"
            name="items"
          />
        </el-checkbox-group>
        
        <el-row>
          <el-button type="primary" @click="addItemConfirm">
            确认
          </el-button>
          <el-button plain @click="addItemCancel">
            取消
          </el-button>
        </el-row>
        <el-button slot="reference" plain>
          添加条件
        </el-button>
      </el-popover>
    </el-row>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        formData: {
          aaa: '',
          bbb: '',
          ccc: '',
          ddd: '',
          eee: '',
          fff: ''
        },
        formList: [
          {
            label: '单位',
            prop: 'aaa',
            type: 'input',
            placeholder: '请输入'
          },
          {
            label: '下拉11',
            prop: 'bbb',
            type: 'select',
            placeholder: '请选择',
            options: [
              {
                label: '是',
                value: '1'
              },
              {
                label: '否',
                value: ''
              }
            ]
          },
          {
            label: '下拉22',
            prop: 'ccc',
            type: 'select',
            placeholder: '请选择',
            options: [
              {
                label: 'I',
                value: '1'
              },
              {
                label: 'II',
                value: '2'
              }
            ]
          },
        ],
        items: [],
        itemsLocal: [],
      }
    },
    computed: {
      formListShow() {
        return this.formList.filter(
          (item) => this.itemsLocal.indexOf(item.label) > -1
        )
      }
    },
    created() {
      this.formList.map((item) => {
        this.itemsLocal.push(item.label)
      })
    },
    methods: {
      popShow() {
        this.items = this.itemsLocal
      },
      addItemConfirm() {
        this.itemsLocal = this.items
        this.$refs.addItemPop.doClose()
      },
      addItemCancel() {

      }
    }
  }
</script>

<style scoped>

</style>