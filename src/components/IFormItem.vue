/* * @Author: Yin Yuhui * @Date: 2020-03-27 00:20:32 * @Last Modified by: Yin
Yuhui * @Last Modified time: 2020-03-27 00:20:32 * 数据校验 展示错误提示 */

<template>
    <div class="form-item">
        <span class="label">{{ label }}:</span>
        <slot></slot>
        <p class="tips" v-show="isError">{{ msg }}</p>
    </div>
</template>

<script>
import schema from 'async-validator'
export default {
    inject: ['form'],
    props: {
        label: {
            type: String,
            default: ''
        },
        prop: {
            type: String,
            default: ''
        }
    },
    data() {
        return {
            msg: '',
            isError: false
        }
    },
    mounted() {
        // 响应input触发的校验
        this.$on('validate', this.validate)
        // 如果有prop属性 说明要校验
        if (this.prop) {
            this.form.$emit('formItemMounted', this)
        }
    },
    methods: {
        validate(value) {
            return new Promise(resolve => {
                // 使用async-validator库进行校验
                const descriptor = {
                    [this.prop]: this.form.rules[this.prop]
                }
                const validator = new schema(descriptor)
                validator.validate({ [this.prop]: value }, errors => {
                    if (errors) {
                        this.msg = errors[0].message
                        this.isError = true
                        // 校验没通过
                        resolve(false)
                    } else {
                        this.msg = ''
                        this.isError = false
                        resolve(true)
                    }
                })
            })
        }
    }
}
</script>

<style scoped>
.form-item {
  position: relative;
}
.tips {
  position: absolute;
  top: 28px;
  left: 110px;
  color: red;
}
.label {
  display: inline-block;
  width: 100px;
  margin: 6px 6px 20px 6px;
  text-align: right;
}
</style>
