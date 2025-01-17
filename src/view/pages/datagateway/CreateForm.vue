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
    :visible.sync="dataGatewayDialogVisible"
    width="30%"
    height="60%"
    top="10vh"
  >
    <el-form
      ref="CreateForm"
      label-position="left"
      label-width="140px"
      style="margin-left: 5%"
      :model="form"
    >
      <el-form-item :label="$t('RULE_ENGINE.DATA_GATEWAY.NAME')" required>
        <el-col :span="20">
          <el-input ref="nameRef" v-model="form.name"></el-input>
        </el-col>
      </el-form-item>

      <el-form-item
        :label="$t('RULE_ENGINE.DATA_GATEWAY.SIGNATURE_MODE')"
        required
      >
        <el-col :span="20">
          <el-select
            style="width: 100%"
            class="el-dark-input"
            v-model="form.signature_mode"
          >
            <el-option
              class="el-dark-input"
              v-for="item in signMethodChoice"
              :key="item"
              :label="item"
              :value="item"
            ></el-option>
          </el-select>
        </el-col>
      </el-form-item>

      <el-form-item
        :label="$t('RULE_ENGINE.DATA_GATEWAY.IP_WHITELIST')"
        required
      >
        <el-col :span="20">
          <el-input
            class="el-dark-input"
            type="textarea"
            ref="nameRef"
            rows="4"
            v-model="form.ip_whitelist"
          ></el-input>
        </el-col>
      </el-form-item>

      <el-form-item
        :label="$t('RULE_ENGINE.DATA_GATEWAY.DEVICE_ACCESS_SCOPE')"
        required
      >
        <el-col :span="20">
          <el-select
            style="width: 100%"
            class="el-dark-input"
            v-model="form.device_access_scope"
          >
            <el-option
              class="el-dark"
              v-for="item in deviceAccessScopeChoice"
              :key="item.value"
              :label="item.name"
              :value="item.value"
            ></el-option>
          </el-select>
        </el-col>
      </el-form-item>

      <el-form-item
        :label="$t('RULE_ENGINE.DATA_GATEWAY.API_ACCESS_SCOPE')"
        required
      >
        <el-col :span="20">
          <el-select
            style="width: 100%"
            class="el-dark-input"
            v-model="form.api_access_scope"
          >
            <el-option
              class="el-dark-input"
              v-for="item in apiAccessScopeChoice"
              :key="item.value"
              :label="item.name"
              :value="item.value"
            ></el-option>
          </el-select>
        </el-col>
      </el-form-item>

      <el-form-item :label="$t('RULE_ENGINE.DATA_GATEWAY.DESCRIPTION')">
        <el-col :span="20">
          <el-input
            class="el-dark-input"
            ref="descRef"
            type="textarea"
            v-model="form.description"
          ></el-input>
        </el-col>
      </el-form-item>

      <div style="display: flex; justify-content: center">
        <el-button
          class="cancel-button"
          type="cancel"
          size="medium"
          plain
          @click="cancelDialog"
          >{{ $t("RULE_ENGINE.DATA_GATEWAY.CANCEL") }}</el-button
        >
        <el-button
          style="margin-left: 10px"
          class="medium"
          type="save"
          size="medium"
          @click="onSubmit"
          >{{ $t("RULE_ENGINE.DATA_GATEWAY.SUBMIT") }}</el-button
        >
      </div>
    </el-form>
  </el-dialog>
</template>

<script>
import {
  updateOpenApiPermission,
  createOpenApiPermission,
} from "@/api/dataGateway";

export default {
  name: "CreateForm",
  props: {
    id: { default: null },
    data: { default: null },
    visible: {
      type: [Boolean],
      default: false,
    },
  },
  computed: {
    dataGatewayDialogVisible: {
      get() {
        return this.visible;
      },
      set(val) {
        this.$emit("update:visible", val);
      },
    },
  },
  watch: {
    visible: {
      handler(newValue) {
        if (newValue && this.id) {
          // 编辑
          this.form = this.data;
        }
      },
    },
  },
  data: () => ({
    // 签名方式选项
    signMethodChoice: ["MD5", "AES-256"],
    // 接口访问范围选项
    apiAccessScopeChoice: [
      { name: "全部", value: "1" },
      { name: "部分", value: "2" },
    ],
    // 设备访问范围选项
    deviceAccessScopeChoice: [
      { name: "全部", value: "1" },
      { name: "部分", value: "2" },
    ],
    // 列表数据
    listData: [],
    form: {
      id: "",
      name: "",
      signature_mode: "",
      ip_whitelist: "",
      device_access_scope: "1",
      api_access_scope: "1",
      created_at: 0,
      description: "",
    },
  }),
  methods: {
    //提交
    onSubmit() {
      let params = JSON.parse(JSON.stringify(this.form));

      if (!this.id) {
        createOpenApiPermission(params).then((res) => {
          if (res.data.code === 200) {
            this.$emit("submit");
            this.listData = [];
            this.dataGatewayDialogVisible = false;
            this.$message({
              message: "新建成功",
              center: true,
              type: "success",
            });
          }
        });
      } else {
        updateOpenApiPermission(params).then((res) => {
          if (res.data.code === 200) {
            this.$emit("submit");
            this.listData = [];
            this.dataGatewayDialogVisible = false;
            this.$message({
              message: "编辑成功",
              center: true,
              type: "success",
            });
          }
        });
      }
    },
    handleClose() {
      this.form = {};
      this.listData = [];
      this.dataGatewayDialogVisible = false;
    },
    cancelDialog() {
      this.form = {};
      this.listData = [];
      this.dataGatewayDialogVisible = false;
    },
    validate() {
      return true;
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
