<template>
  <div class="login">
    <a-form
        :model="formState"
        name="basic"
        :label-col="{ span: 8 }"
        :wrapper-col="{ span: 16 }"
        autocomplete="off"
        @finish="onFinish"
    >
        <a-form-item
        label="Username"
        name="username"
        :rules="[{ required: true, message: '请输入用户名' }]"
        >
        <a-input v-model:value="formState.username" />
        </a-form-item>

        <a-form-item
        label="Password"
        name="password"
        :rules="[{ required: true, message: '请输入密码' }]"
        >
        <a-input-password v-model:value="formState.password" />
        </a-form-item>

        <a-form-item :wrapper-col="{ offset: 8, span: 16 }">
        <a-button type="primary" html-type="submit">登录</a-button>
        </a-form-item>
    </a-form>
  </div>
</template>
<script lang="ts">
import { defineComponent, reactive } from 'vue';

interface FormState {
  username: string;
  password: string;
}
import { message } from 'ant-design-vue';

export default defineComponent({
  setup() {
    const formState = reactive<FormState>({
      username: '',
      password: '',
    });
    const onFinish = (values: any) => {
      window.fetch('/api/user/login', {
        method: 'POST',
        body: JSON.stringify(values),
        headers: {
            'Content-Type': 'application/json',
        }
      }).then(res => {
        console.log(res)
        if (
            values.username === 'jack' && values.password === 'redballoon'
        ) {
            return true
        }
        return false
      }).then(result => {
        if (result) {
            message.success('登录成功')
        } else {
            message.error('密码错误')
        }
      }).catch(e => {
        console.log(e)
      })
    };
    return {
      formState,
      onFinish,
    };
  },
});
</script>

<style>

</style>
