<template>
  <el-dialog
    :title="
      id
        ? $t('RULE_ENGINE.DATA_GATEWAY.EDIT')
        : $t('RULE_ENGINE.DATA_GATEWAY.CREATE')
    "
    class="el-dark-dialog"
    :close-on-click-modal="false"
    :before-close="handleClose"
    :visible.sync="apiDialogVisible"
    width="40%"
    height="60%"
    top="10vh"
  >
    <div class="rounded p-4 card">
      <el-row type="flex" :gutter="20" class="pt-3 pb-4 px-3">
        <el-col :span="24">
          <el-select
            style="width: 20%"
            class="el-dark-input"
            placeholder="全部"
          >
            <!-- <el-option
              class="el-dark"
              value="-1"
              style="display: none;"
            ></el-option>
            <el-option
              class="el-dark"
              v-for="item in deviceAccessScopeChoice"
              :key="item.value"
              :label="item.name"
              :value="item.value"
            ></el-option> -->
          </el-select>

          <el-input
            v-model="keyword"
            style="width: 20%; margin-left: 10px"
            :placeholder="$t('RULE_ENGINE.DATA_GATEWAY.INTERFACE_NAME')"
            clearable
          >
          </el-input>
        </el-col>
      </el-row>

      <el-table :data="filteredData" v-loading="loading">
        <el-table-column
          type="selection"
          :label="$t('RULE_ENGINE.DATA_GATEWAY.INTERFACE_NAME')"
          prop="interface_name"
        ></el-table-column>

        <el-table-column
          :label="$t('RULE_ENGINE.DATA_GATEWAY.INTERFACE_NAME')"
          prop="interface_name"
        ></el-table-column>

        <el-table-column
          :label="$t('RULE_ENGINE.DATA_GATEWAY.URL')"
          prop="url"
        ></el-table-column>

        <el-table-column
          :label="$t('RULE_ENGINE.DATA_GATEWAY.INTERFACE_TYPE')"
          prop="interface_type"
        ></el-table-column>

        <el-table-column
          :label="$t('RULE_ENGINE.DATA_GATEWAY.INTERFACE_DESCRIPTION')"
          prop="interface_description"
        ></el-table-column>
      </el-table>

      <div class="text-right py-3">
        <el-pagination
          background
          layout="total, prev, pager, next"
          :total="data_count"
          :current-page.sync="page"
          :page-size="page_size"
          @current-change="page_change"
        ></el-pagination>
      </div>

      <div>
        <el-button size="mini" type="indigo" @click="handleClose">{{
          $t("RULE_ENGINE.DATA_GATEWAY.CLOSE")
        }}</el-button>
      </div>
    </div>
  </el-dialog>
</template>

<script>
export default {
  name: "ApiAccessScopeForm",
  props: {
    id: { default: null },
    data: { default: [] },
    tableData: { default: null },
    keyword: { default: "" },
    visible: {
      type: [Boolean],
      default: false,
    },
  },
  computed: {
    apiDialogVisible: {
      get() {
        return this.visible;
      },
      set(val) {
        this.$emit("update:visible", val);
      },
    },
  },
  watch: {
    keyword() {
      let data = this.listData;
      if (this.keyword) {
        data = data.filter((item) =>
          item.interface_name.includes(this.keyword)
        );
      }
      this.page = 1;
      this.filteredData = data.slice(
        (this.page - 1) * this.page_size,
        this.page * this.page_size
      );
      this.data_count = data.length;
    },
    visible: {
      handler(newValue) {
        if (newValue && this.id) {
          // 编辑
          this.page = 1;
          this.get_interface_data();
          this.filteredData = this.listData.slice(
            (this.page - 1) * this.page_size,
            this.page * this.page_size
          );
          this.data_count = this.listData.length;
        }
      },
    },
  },
  data: () => ({
    page: 0,
    page_size: 5,
    data_count: 0,
    // 列表数据
    listData: [],
    filteredData: [],
    form: {
      id: "",
      name: "",
      app_key: "",
      signature_mode: "",
      ip_whitelist: "",
      device_access_scope: 0,
      api_access_scope: 0,
      created_at: 0,
      tenant_id: "",
      description: "",
    },
  }),
  methods: {
    page_change(val) {
      this.page = val;
      console.log("page_change: ");
      console.log(this.page);

      let data = this.listData;
      if (this.keyword) {
        data = data.filter((item) =>
          item.interface_name.includes(this.keyword)
        );
      }
      this.filteredData = data.slice(
        (this.page - 1) * this.page_size,
        this.page * this.page_size
      );
      this.data_count = data.length;
    },
    get_interface_data() {
      this.listData = [
        {
          interface_name: "查看",
          url: "asd/asdasd",
          interface_type: "温湿度",
          interface_description: "温湿度",
        },
        {
          interface_name: "查看4",
          url: "asd/asdasd111",
          interface_type: "温湿度",
          interface_description: "温湿度",
        },
        {
          interface_name: "查看44",
          url: "asd/asdasd111",
          interface_type: "温湿度",
          interface_description: "温湿度",
        },
        {
          interface_name: "查看4444",
          url: "asd/asdasd111",
          interface_type: "温湿度",
          interface_description: "温湿度",
        },
        {
          interface_name: "查看411",
          url: "asd/asdasd111",
          interface_type: "温湿度",
          interface_description: "温湿度",
        },
        {
          interface_name: "查看412311",
          url: "asd/asdasd111",
          interface_type: "温湿度",
          interface_description: "温湿度",
        },
        {
          interface_name: "查看412311",
          url: "asd/asdasd111",
          interface_type: "温湿度",
          interface_description: "温湿度",
        },
        {
          interface_name: "查看4",
          url: "asd/asdasd111",
          interface_type: "温湿度",
          interface_description: "温湿度",
        },
        {
          interface_name: "查看5",
          url: "asd/asdasd222",
          interface_type: "温湿度",
          interface_description: "温湿度",
        },
      ];
    },
    handleClose() {
      this.page = 1;
      this.keyword = "";
      this.form = {};
      this.listData = [];
      this.apiDialogVisible = false;
    },
    cancelDialog() {
      this.page = 1;
      this.keyword = "";
      this.form = {};
      this.listData = [];
      this.apiDialogVisible = false;
    },
  },
};
</script>

<style scoped lang="scss">
.code-editor-label {
  color: #fff;
  margin-top: 10px;
}
::v-deep .code_editor .code_area textarea {
  overflow-y: auto;
}
.dialog-box {
  border: 1px solid #e9e9eb;
  padding: 10px;
  cursor: pointer;
  p {
    margin-bottom: 0;
  }
}
.dialog-box:last-child {
  margin-top: 20px;
}

.dialog-border {
  border: 1px solid #5867dd;
  padding: 10px;
  cursor: pointer;
  p {
    margin-bottom: 0;
  }
}

.dialog-border:last-child {
  margin-top: 20px;
}

.list_box {
  margin-top: 20px;
  .item {
    display: flex;
    align-items: center;
    height: 60px;
    justify-content: space-between;
    border-bottom: 1px solid #ccc;
    .flex_full {
      flex: 1;
    }
    &:first-child {
      border-top: 1px solid #ccc;
    }
  }
}
</style>
