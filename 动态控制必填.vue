<template>
  <el-form :model="form" :rules="rules" ref="formRef">
    <el-form-item label="选择类型" prop="type">
      <el-radio-group v-model="form.type">
        <el-radio label="A">类型 A</el-radio>
        <el-radio label="B">类型 B</el-radio>
      </el-radio-group>
    </el-form-item>
    <el-form-item v-if="showUsername" label="用户名" prop="username">
      <el-input v-model="form.username"></el-input>
    </el-form-item>
    <el-form-item v-if="showPassword" label="密码" prop="password">
      <el-input v-model="form.password" type="password"></el-input>
    </el-form-item>
    <el-form-item>
      <el-button type="primary" @click="onSubmit">提交</el-button>
    </el-form-item>
  </el-form>
</template>
<script setup>
import { ref, watch } from 'vue';
import { ElMessage } from 'element-plus';

const form = ref({
  type: 'A',
  username: '',
  password: ''
});

const rules = ref({
  username: [
    { required: false, message: '请输入用户名', trigger: 'blur' }
  ],
  password: [
    { required: false, message: '请输入密码', trigger: 'blur' }
  ]
});

const showUsername = ref(true);
const showPassword = ref(false);

watch(() => form.value.type, (newType) => {
  if (newType === 'A') {
    showUsername.value = true;
    showPassword.value = false;
    rules.value.username[0].required = true;
    rules.value.password[0].required = false;
  } else if (newType === 'B') {
    showUsername.value = true;
    showPassword.value = true;
    rules.value.username[0].required = true;
    rules.value.password[0].required = true;
  }
}, { immediate: true });

const formRef = ref(null);

const onSubmit = () => {
  formRef.value.validate((valid) => {
    if (valid) {
      ElMessage.success('提交成功');
    } else {
      ElMessage.error('请检查输入信息');
    }
  });
};
</script>
<style scoped>
.el-form {
  max-width: 500px;
  margin: 0 auto;
}
</style>

