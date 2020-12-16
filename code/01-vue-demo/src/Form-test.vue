<template>
  <lbj-form class="form" ref="form" :model="user" :rules="rules">
    <lbj-form-item label="用户名" prop="username">
      <!-- <lbj-input v-model="user.username"></lbj-input> -->
      <lbj-input :value="user.username" @input="user.username = $event" placeholder="请输入用户名"></lbj-input>
    </lbj-form-item>
    <lbj-form-item label="密码" prop="password">
      <lbj-input type="password" v-model="user.password"></lbj-input>
    </lbj-form-item>
    <lbj-form-item>
      <lbj-button type="primary" @click="login">登 录</lbj-button>
    </lbj-form-item>
  </lbj-form>
</template>

<script>
import LbjForm from './form/Form'
import LbjFormItem from './form/FormItem'
import LbjInput from './form/Input'
import LbjButton from './form/Button'
export default {
  components: {
      LbjForm,
      LbjFormItem,
      LbjInput,
      LbjButton
  },
  data () {
    return {
      user: {
        username: '',
        password: ''
      },
      rules: {
        username: [
          {
            required: true,
            message: '请输入用户名'
          }
        ],
        password: [
          {
            required: true,
            message: '请输入密码'
          },
          {
            min: 6,
            max: 12,
            message: '请输入6-12位密码'
          }
        ]
      }
    }
  },
  methods: {
    login () {
      console.log('button')
      this.$refs.form.validate(valid => {
        if (valid) {
          alert('验证成功')
        } else {
          alert('验证失败')
          return false
        }
      })
    }
  }
}
</script>

<style>
  .form {
    width: 30%;
    margin: 150px auto;
  }
</style>
