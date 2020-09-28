<template>
  <div class="daiContent" v-loading="loading">
    <div class="titleCb">
      <span class="rightBat">
        <el-button
          size="mini"
          @click="$router.push({ path: '/AuditTemplate' })"
          >{{
            $root.langs["QualityManageSystem.Exceptionmanagement.Cancel"]
          }}</el-button
        >
        <el-button size="mini" type="primary" @click="save">{{
          $root.langs["QualityManageSystem.Exceptionmanagement.Submit"]
        }}</el-button>
      </span>
      <el-breadcrumb separator-class="el-icon-arrow-right">
        <el-breadcrumb-item :to="{ path: '/AuditTemplate' }">{{
          $root.langs.IPQCAuditTemplate
        }}</el-breadcrumb-item>
        <el-breadcrumb-item>{{ $root.langs.Addtemplate }}</el-breadcrumb-item>
      </el-breadcrumb>
    </div>
    <div class="boxDAI">
      <h4 class="title">{{ $root.langs.Addtemplate }}</h4>
      <div class="upLoad">
        <el-form
          label-position="left"
          style="display: flex;flex-wrap:wrap;"
          :model="form"
          ref="form"
          :rules="rules"
          label-width="170px"
        >
          <el-form-item
            style="width: 45%"
            :label="$root.langs.Templatename + '：'"
            prop="name"
          >
            <el-input
              :placeholder="
                $root.langs['QualityManageSystem.Exceptionmanagement.Enter']
              "
              v-model.trim="form.name"
            ></el-input>
          </el-form-item>
          <el-form-item
            style="width: 45%;margin-left: 5%"
            :label="
              $root.langs[
                'QualityManageSystem.Exceptionmanagement.Description'
              ] + '：'
            "
            prop="description"
          >
            <el-input
              :placeholder="
                $root.langs['QualityManageSystem.Exceptionmanagement.Enter']
              "
              v-model.trim="form.description"
            ></el-input>
          </el-form-item>
          <el-form-item
            style="width: 100%"
            :label="
              $root.langs[
                'QualityManageSystem.Exceptionmanagement.Formuploading'
              ] + '：'
            "
            prop="productName"
          >
            <el-input
              :placeholder="
                $root.langs['QualityManageSystem.Exceptionmanagement.Enter']
              "
              :disabled="true"
              class="upInput"
              v-model.trim="value"
            ></el-input>
            <el-upload
              accept=".xlsx, .xls"
              class="upLoadBat"
              action=""
              :before-upload="beforeUpload"
              :multiple="false"
              :show-file-list="false"
            >
              <el-button type="primary">{{
                $root.langs["QualityManageSystem.Document.Upload"]
              }}</el-button>
            </el-upload>
          </el-form-item>
        </el-form>
      </div>
      <h4 class="title">{{ $root.langs.Templateinformationlist }}</h4>
      <el-table stripe :data="form.auditItemBeanList">
        <el-table-column
          :label="
            $root.langs['QualityManageSystem.Problembacktracking.SerialNumber']
          "
          type="index"
          align="center"
          width="100"
          class-name="indexAlign"
        ></el-table-column>
        <el-table-column
          prop="itemType"
          :label="$root.langs['QualityManageSystem.IPQCAudit.Project']"
        ></el-table-column>
        <el-table-column
          prop="itemName"
          :label="$root.langs.Subproject"
        ></el-table-column>
        <el-table-column
          prop="alarmType"
          :label="
            $root.langs['QualityManageSystem.Exceptionmanagement.Operating']
          "
          width="130"
          class-name="indexAlign"
        >
          <template slot-scope="scope">
            <i
              class="el-icon-delete"
              :title="
                $root.langs['QualityManageSystem.Exceptionmanagement.Delete']
              "
              @click="del(scope.$index)"
            ></i>
          </template>
        </el-table-column>
      </el-table>
    </div>
  </div>
</template>
<script>
import XLSX from "xlsx";
export default {
  data() {
    return {
      value: "",
      loading: false,
      form: {
        name: "",
        description: "",
        auditItemBeanList: []
      },
      rules: {
        name: [
          { required: true, message: this.$root.langs.Input, trigger: "blur" },
          {
            max: 50,
            message: this.$root.langs.Maximumcharacters50,
            trigger: "blur"
          }
        ],
        description: [
          { required: true, message: this.$root.langs.Input, trigger: "blur" },
          {
            max: 50,
            message: this.$root.langs.Maximumcharacters50,
            trigger: "blur"
          }
        ]
      }
    };
  },
  methods: {
    beforeUpload(file) {
      let that = this;
      if (!/\.xlsx?$/.test(file.name)) {
        this.$errorD(
          this.$root.langs[
            "QualityManageSystem.Exceptionmanagement.ecorrecttemplatefile"
          ]
        );
        return;
      } else if (file.size > 2 * 1024 * 1024) {
        this.$errorD(this.$root.langs.Filesizecannotexceed2MB);
        return;
      }
      that.value = file.name;
      let reader = new FileReader();
      reader.onload = function(e) {
        try {
          let data = e.target.result;
          let wb = XLSX.read(data, { type: "binary" });
          let persons = [];
          let fromTo = "";
          for (let sheet in wb.Sheets) {
            if (wb.Sheets.hasOwnProperty(sheet)) {
              fromTo = wb.Sheets[sheet]["!ref"];
              persons = persons.concat(
                XLSX.utils.sheet_to_json(wb.Sheets[sheet])
              );
              break;
            }
          }
          if (!persons[0].itemType) {
            that.$errorD(
              that.$root.langs[
                "QualityManageSystem.Exceptionmanagement.ecorrecttemplatefile"
              ]
            );
            that.value = "";
            return;
          }
          that.form.auditItemBeanList = persons;
        } catch (err) {
          that.$errorD(
            that.$root.langs[
              "QualityManageSystem.Exceptionmanagement.ecorrecttemplatefile"
            ]
          );
        }
      };
      reader.readAsBinaryString(file);
      return false;
    },
    del(index) {
      this.$c_confirm(
        () => {
          this.form.auditItemBeanList.splice(index, 1);
        },
        this.$root.langs["QualityManageSystem.QualityAlert.Prompt"],
        this.$root.langs[
          "QualityManageSystem.Exceptionmanagement.Deletetherecordornot"
        ]
      );
    },
    async save() {
      let that = this;
      that.loading = true;
      that.$refs.form.validate(async valid => {
        if (valid) {
          let res = await this.$post(
            `${Jw.ipqcaudit}/ipqcaudit/saveAuditTemplate`,
            this.form
          );
          if (+res.code === 0) {
            that.$successD();
            setTimeout(function() {
              that.$router.push({ path: "/AuditTemplate" });
            }, 1000);
          }
          that.loading = false;
        } else {
          that.loading = false;
          return false;
        }
      });
    }
  }
};
</script>
<style scoped>
.titleCb {
  height: 14px;
  margin-bottom: 20px;
}
.rightBat {
  float: right;
  margin-top: -7px;
}
.box {
  background: #fff;
  border: 1px solid #eaeaea;
  margin-bottom: 20px;
  min-height: calc(100vh - 200px);
}
.upLoad {
  padding: 30px 30px 0px 30px;
}
.upInput {
  width: 400px;
}
.upLoadBat {
  display: inline-block;
  height: 32px;
  line-height: 30px;
  vertical-align: top;
}
.bat {
  cursor: pointer;
}
.bat:last-child {
  color: #ff0000;
  margin-left: 10px;
}
.bat:hover {
  color: #147eef;
}
</style>
<style>
.indexAlign div {
  text-align: center;
}
</style>