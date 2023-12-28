<template>
  <el-popover
    placement="bottom"
    popper-class="cusPopover"
    trigger="click"
    :value="popoverShow"
    @show="onPopoverShow"
  >
    <div class="drop_box">
      <div class="run-group-tree">
        <div class="topContent">
          <el-input
            v-model="filterText"
            placeholder="请输入"
            prefix-icon="el-icon-search"
            clearable
          />
        </div>
        <div class="treeContent">
          <el-tree 
            ref="tree"
            node-key="id"
            lazy
            :check-strictly="checkStrictly"
            :props="treeProps"
            :show-checkbox="multiple"
            :highlight-current="!multiple"
            :filter-node-method="filterNode"
            @node-click="handleNodeClick"
            @check-change="handleCheckChange"
          />
        </div>
      </div>
    </div>
    <el-select
      slot="reference"
      v-model="optionData.name"
      placeholder="请选择"
      :multiple="multiple"
      popper-class="cusSelect"
      collapse-tags
      clearable
      @clear="selectClear"
      @remove-tag="removeTag"
    />
  </el-popover>
</template>

<script>
  export default {
    name: 'SelectTree',
    props: {
      popoverShow: {
        type: Boolean,
        default: false
      },
      value: {
        type: [Array, String],
        default: () => {
          return []
        }
      },
      multiple: {
        type: Boolean,
        default: false
      },
      filterable: {
        type: Boolean,
        default: false
      },
      checkStrictly: {
        type: Boolean,
        default: false
      }
    },
    data() {
      return {
        treeProps: {
          isLeaf: 'leaf'
        },
        opitonData: {
          id: '',
          name: ''
        },
        filterText: ''
      }
    },
    watch: {
      value: {
        handler(val) {
          if (val) {
            this.init(val)
          }
        },
        deep: true,
        immediate: true
      },
      filterText: {
        handler(val) {
          if (this.$refs.tree) {
            this.$refs.tree.filter(val)
          }
        },
        deep: true,
        immediate: true
      }
    },
    methods: {
      onPopoverShow() {
        this.$emit('onPopoverShow', 'true')
      },
      async loadNode(node, resolve) {
        this.$emit('loadNode', { node, resolve })
      },
      handleNodeClick(data, node) {
        if (this.multiple) {
          return
        }
        this.$emit('checkChange', { data, node })
      },
      handleCheckChange(data, checked) {
        this.$emit('checkChange', { data, checked })
      },
      removeTag(val) {
        let checkedNodes = this.$refs.tree.getCheckedNodes()
        let data = checkedNodes.filter(e => e.label == val)
        let node = this.$refs.tree.getNode(data[0])
        let removeNodeIds = this.getCheckedIds([node])
        let hasCheckedNodes = checkedNodes.filter(e => removeNodeIds.indexOf(e.id) == -1)
        let checkedIds = hasCheckedNodes.length ? hasCheckedNodes.map(e => e.id) : []
        this.$refs.tree.setCheckedKeys(checkedIds)
      },
      getCheckedIds(checkedArr, childNodesId = []) {
        checkedArr.map(item => {
          childNodesId = [...childNodesId, item.data.id]
          if (item.childNodes.length) {
            let checkedIds = this.getCheckedIds(item.childNodes)
            childNodesId = [...childNodesId, ...checkedIds]
          }
        })
        return childNodesId
      },
      init(val) {
        if (this.multiple) {
          const arr = val && val.length ? val.toString().split(',') : []
          this.$nextTick(() => {
            if (arr.length) {
              let nodes = this.$refs.tree.getCheckedNodes()
              let checkedNodes = nodes.filter(e => arr.indexOf(e.id) > -1)
              let ids = checkedNodes.map(item => item.id)
              this.optionData.id = ids
              this.opitonData.name = checkedNodes.map(item => item.label)
            } else {
              this.opitonData.id = ''
              this.opitonData.name = ''
            }
          })
        }
      },
      selectClear() {
        this.opitonData.id = ''
        this.opitonData.name = ''
        this.$refs.tree.setCheckedNodes([])
        this.$emit('clear')
      },
      filterNode(value, data) {
        if (!value) {
          return
        }
        return data.label.indexOf(value) !== -1
      }
    }
  }
</script>

<style lang="scss" scoped>
.drop_box {
  width: 100%;
  height: 335px;
}
.run-group-tree {
  height: 100%;
  overflow: hidden;
}
.topContent {
  display: flex;
  justify-content: space-between;
  margin-bottom: 10px;
  .el-input {
    margin-right: 10px;
  }
}
.treeContent {
  height: calc(100% - 42px);
  overflow-y: auto;
}
::v-deep .el-select__tags-text {
  max-height: 180px !important;
}
</style>
<style lang="scss">
.el-select-dropdown.el-popper.cusSelect {
  display: none !important;
}
.el-popover.el-popper.cusPopover {
  min-width:  293px;
}
</style>