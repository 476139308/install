<template>
  <el-container>
    <el-header>远程安装参数</el-header>
    <el-switch
      property=""
      style="display: block"
      v-model="mode"
      active-color="#13ce66"
      inactive-color="#ff4949"
      active-text="用户名密码模式"
      inactive-text="SSH密码模式">
    </el-switch>
    <el-main>
      <el-form :model="ruleForm" status-icon :rules="rules" ref="ruleForm" label-width="100px" class="demo-ruleForm">
        <el-form-item label="远程Ip" prop="targetIp">
          <el-input type="text" v-model="ruleForm.targetIp" autocomplete="on"></el-input>
        </el-form-item>
        <el-form-item label="端口号" prop="port">
          <el-input type="number" v-model="ruleForm.port" autocomplete="on"></el-input>
        </el-form-item>
        <el-form-item label="用户名" prop="username" >
          <el-input type="text" v-model="ruleForm.username" autocomplete="on"></el-input>
        </el-form-item>
        <el-form-item label="密码" prop="password" v-if="mode">
          <el-input type="password" v-model="ruleForm.password" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="SSH" prop="password" v-if="!mode">
          <el-input type="text" v-model="ruleForm.ssh" autocomplete="on"></el-input>
        </el-form-item>
      </el-form>
    </el-main>
    <el-form>
      <el-form-item>
        <el-button type="primary" @click="submitForm('ruleForm')">提交</el-button>
        <el-button @click="resetForm('ruleForm')">重置</el-button>
      </el-form-item>
    </el-form>
  </el-container>

</template>
<script>
export default {
  data() {
    var validateTargetIp = (rule, value, callback) => {
      if (value === '') {
        callback(new Error('请输入ip地址'));
      } else {
        // 校验IP是否符合规则
        var regEx = /,/g;
        var ipList = value.toString().replace(regEx, ',').split(',');
        var reg =
          /^(\d{1,2}|1\d\d|2[0-4]\d|25[0-5])\.(\d{1,2}|1\d\d|2[0-4]\d|25[0-5])\.(\d{1,2}|1\d\d|2[0-4]\d|25[0-5])\.(\d{1,2}|1\d\d|2[0-4]\d|25[0-5])$/;
        for (var i in ipList) {
          if (!reg.test(ipList[i])) {
            return callback(new Error('请输入节点ip地址，多节点用,分隔'));
          } else {
            callback();
          }
        }

      }
    };
    var validatePort = (rule, value, callback) => {
      if (value === '') {
        callback(new Error('请输入端口号'));
      } else {
        var reg = /^(([0-9]|[1-9]\d{1,3}|[1-5]\d{4}|6[0-5]{2}[0-3][0-5]))$/;
        if (value === '') {
          return callback(new Error('请输入端口号'));
        } else if (reg.test(value) === false) {
          callback(new Error('请输入正确的端口号'));
        } else {
          callback();
        }
      }
    };

    return {
      mode: true,
      ruleForm: {
        targetIp: '',
        port: '',
        username: '',
        password: '',
        ssh: ''
      },
      rules: {
        targetIp: [
          { validator: validateTargetIp, trigger: 'blur' }
        ],
        port: [
          { validator: validatePort, trigger: 'blur' }
        ]
      }
    };
  },
  methods: {
    submitForm(formName) {

      let _this = this
      this.$axios
        .post('http://localhost:8080/login',_this.ruleForm)
        .then(function (response) {
          console.log(response);
          this.$router.push("/about")
        })
    },
    resetForm(formName) {
      this.$refs[formName].resetFields();
    }
  }
}
</script>

<style>
.el-header, .el-footer {
  background-color: #B3C0D1;
  color: #333;
  text-align: center;
  line-height: 60px;
}

.el-aside {
  background-color: #D3DCE6;
  color: #333;
  text-align: center;
  line-height: 200px;
}

.el-main {
  background-color: #E9EEF3;
  color: #333;
  text-align: center;
  line-height: 160px;
}

body > .el-container {
  margin-bottom: 40px;
}

.el-container:nth-child(5) .el-aside,
.el-container:nth-child(6) .el-aside {
  line-height: 260px;
}

.el-container:nth-child(7) .el-aside {
  line-height: 320px;
}
</style>
