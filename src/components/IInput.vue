/* * @Author: Yin Yuhui * @Date: 2020-03-26 21:17:27 * @Last Modified by: Yin
Yuhui * @Last Modified time: 2020-03-26 21:17:27 * Input
数据双向绑定，变化时通知 formItem 校验 */

<template>
    <input :type="type" :value="value" @input="handleInput" />
</template>

<script>
export default {
    props: {
        type: {
            type: String,
            default: 'text'
        },
        value: {
            type: [String, Number],
            default: ''
        }
    },

    methods: {
        handleInput(e) {
            const value = e.target.value

            // 数据双向绑定
            this.$emit('input', value)

            // 通知父组件数据变化
            this.$parent.$emit('validate', value)
        }
    }
}
</script>
