<template>
  <div>
    <span class="kv-description" v-if="description">
      {{description}}
    </span>
    <div class="kv-row" v-for="(item, index) in items" :key="index">
      <el-row type="flex" :gutter="20" justify="space-between" align="middle">
        <el-col :span="11">
          <el-input v-model="item.key" placeholder="Key" size="small" maxlength="100" @change="check"/>
        </el-col>
        <el-col :span="11">
          <el-input v-model="item.value" placeholder="Value" size="small" maxlength="100" @change="check"/>
        </el-col>
        <el-col :span="1">
          <el-button size="mini" class="el-icon-delete-solid" circle @click="remove(index)"/>
        </el-col>
      </el-row>
    </div>
  </div>
</template>

<script>
  export default {
    name: "MsApiKeyValue",

    props: {
      description: String,
      items: Array
    },

    methods: {
      create: function () {
        return {
          key: "",
          value: ""
        }
      },
      remove: function (index) {
        this.items.splice(index, 1);
        if (this.items.length === 0) {
          this.items.push(this.create());
        }
      },
      check: function () {
        let isNeedCreate = true;
        let removeIndex = -1;
        this.items.forEach((item, index) => {
          if (item.key === "" && item.value === "") {
            // 多余的空行
            if (index !== this.items.length - 1) {
              removeIndex = index;
            }
            // 没有空行，需要创建空行
            isNeedCreate = false;
          }
        });
        if (isNeedCreate) {
          this.items.push(this.create());
        }
        if (removeIndex !== -1) {
          this.remove(removeIndex);
        }
        // TODO 检查key重复
      }
    },

    created() {
      if (this.items.length === 0) {
        this.items.push(this.create());
      }
    }
  }
</script>

<style scoped>
  .kv-description {
    font-size: 14px;
  }

  .kv-row {
    margin-top: 10px;
  }
</style>