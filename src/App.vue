<template>
    <div id="app">
        <i-form :model="form" :rules="rules" ref="form">
            <i-form-item label="姓名" prop="name">
                <i-input v-model="form.name"></i-input>
            </i-form-item>
            <i-form-item label="年龄">
                <i-input v-model="form.age"></i-input>
            </i-form-item>
            <i-form-item label="密码" prop="pwd">
                <i-input text="password" v-model="form.pwd"></i-input>
            </i-form-item>
        </i-form>
        <button class="btn" @click="submit">提交</button>
    </div>
</template>

<script>
import IForm from './components/IForm.vue'
import IFormItem from './components/IFormItem.vue'
import IInput from './components/IInput'
export default {
    name: 'App',
    components: {
        IForm,
        IFormItem,
        IInput
    },
    data() {
        return {
            form: {
                name: 'yyh',
                age: 18
            },
            test: '',
            rules: {
                name: [
                    {
                        required: true,
                        message: '请输入姓名'
                    },
                    {
                        min: 4,
                        max: 8,
                        message: '姓名必须为4-8个字'
                    }
                ],
                pwd: {
                    required: true,
                    min: 6,
                    max: 10,
                    message: '密码长度为6-10'
                }
            }
        }
    },
    methods: {
        submit() {
            this.$refs.form.validate(result => {
                console.log(result)
            })
        }
    }
}
</script>

<style>
* {
  padding: 0;
  margin: 0;
}
#app {
  padding-top: 20px;
}
.btn {
  margin-left: 216px;
}
</style>
