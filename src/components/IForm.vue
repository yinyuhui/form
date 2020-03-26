/* * @Author: Yin Yuhui * @Date: 2020-03-27 00:23:17 * @Last Modified by: Yin
Yuhui * @Last Modified time: 2020-03-27 00:23:17 * 绑定表单数据和校验规则 */

<template>
    <div>
        <slot></slot>
    </div>
</template>

<script>
export default {
    // 要写成函数返回对象 直接写成对象是undefined
    provide() {
        return {
            form: this
        }
    },
    props: {
        // formItem 校验用
        rules: {
            type: Object
        },
        // 用于提交表单校验时 获取每一项值， formItem 的 validate 方法要传参
        model: {
            type: Object
        }
    },
    created() {
        this.fields = []
        this.$on('formItemMounted', item => {
            // 存一下要校验的表单项
            this.fields.push(item)
        })
    },
    methods: {
        // form上的validate方法，提交表单时用
        async validate(cb) {
            // 获取每个表单项的校验promise
            const tasks = this.fields.map(item => {
                return item.validate(this.model[item.prop])
            })

            // 拿到每一项的校验结果
            const results = await Promise.all(tasks)

            // 执行回调 参数为是否全部表单项通过校验
            cb(!results.includes(false))
        }
    }
}
</script>
