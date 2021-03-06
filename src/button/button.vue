<template>
    <!-- $  ('click') 表示按钮被点击就触发一个事件，事件名称叫click -->
    <button class="c-button" :class="[{[`icon-${iconPosition}`]: true, 'in-circle': circle, 'in-disabled': disabled, 'in-round': round, [`in-button-${type}`]:type}]"
        @click="$emit('click')"> 
        <c-icon v-if="icon && !loading" class="icon" :name="icon" :class="{[`${rmIconMargin}`]: true}"></c-icon>
        <c-icon v-if="loading" class="loading icon" name="loading" :class="{[`${rmIconMargin}`]: true}"></c-icon>
        <span class="content">
            <slot></slot>
        </span>
    </button>
</template>
<script>
    import Icon from '../icon'
    export default {
        name: 'CareButton',
        components: {
            'c-icon': Icon
        },
        props: {
            icon: {},
            loading: {
                type: Boolean,
                default: false
            },
            circle: {
                type: Boolean,
                default: false
            },
            disabled: {
                type: Boolean,
                default: false
            },
            round: {
                type: Boolean,
                default: false
            },
            type: {
                type: String, // 类型
                default: null, // 默认值
                // 属性的检查器
                validator(value){
                    return value?value === 'primary' || value === 'warning': null
                }
            },
            iconPosition: {
                type: String, // 类型
                default: 'left', // 默认值
                // 属性的检查器
                validator(value){
                    return value === 'left' || value === 'right'
                }
            }
        },
        data() {
            return {
                iconClass: false
            }
        },
        computed: {
            rmIconMargin(){
                if(!this.$slots.default||this.$slots.default.length<=0){
                    return 'rm-margin'
                }
            }
        },
        mounted(){
            this.$nextTick(()=>{
                if(this.$el.children[1]&&this.$el.children[1].innerHTML&&!this.$el.children[1].innerHTML) {
                    this.$el.children[0].classList.add('rm-margin')
                }
            })
        }
    }
</script>
<style lang="scss" scoped>
    $button-height: 32px;
    $font-size: 14px;
    $button-bg: #ffffff;
    $button-bg-hover: #e6eef8;
    $button-border-hover: #b3cceb;
    $border-radius: 4px;
    $color: #303133;
    $border-color: #c0c4cc;
    // 蓝色
    $primary-color: #05b;
    $primary-light-color: #3377c9;
    $primary-dark-color: #004da8;

    // 警告色
    $warning-color: #ff4d4f;
    $warning-light-color: #ff7875;
    $warning-dark-color: #d9363e;
    @keyframes spin {
        0% { transform: rotate(0deg) }
        100% { transform: rotate(360deg) }
    }
    .c-button {
        transition: all 0.3s;
        color: $color;
        font-size: $font-size;
        height: $button-height;
        padding: 0 1em;
        border-radius: $border-radius;
        border: 1px solid $border-color;
        background: $button-bg;
        cursor: pointer;
        display: inline-flex;
        justify-content: center;
        align-items: center;
        vertical-align: middle;
        &:hover, &:focus {
            fill: $primary-color;
            color: $primary-color;
            background-color: $button-bg-hover;
            border-color: $button-border-hover;
        }
        &:active {
            fill: $primary-dark-color;
            color: $primary-dark-color;
            border-color: $primary-dark-color;
        }
        &:focus {
            outline: none;
        }
        > .icon { order: 1; margin-right: .3em; }
        > .content { order: 2; }
        &.icon-left {
            .rm-margin {
                margin-right: 0;
            }
        }
        &.icon-right {
            > .icon { order: 2; margin-right: 0; margin-left: .3em; }
            > .content { order: 1; }
            > .rm-margin {
                margin-left: 0;
            }
        }
        .loading {
            margin-right: .3em;
            animation: spin 1.2s infinite linear;
        }
        &.in-button-primary {
            fill: #fff;
            color: #fff;
            background-color: $primary-color;
            border-color: $primary-color;
            &:hover, &:focus {
                background: $primary-light-color;
                border-color: $primary-light-color;
                color: #fff;
            }
            &:active {
                background: $primary-dark-color;
                border-color: $primary-dark-color;
                color: #fff;
                outline: none;
            }
            &.in-disabled {
                cursor: default;
                user-select: none;
                color: #f1f3f4;
                background-color: $primary-light-color;
                border-color: $primary-light-color;
                &:hover, &:focus {
                    background-color: $primary-light-color;
                }
                &:active {
                    border-color: $primary-light-color;
                }
                .icon {
                    fill: #f1f3f4;
                }
            }
        }
        &.in-button-warning {
            fill: #fff;
            color: #fff;
            background: $warning-color;
            border-color: $warning-color;
            &:hover, &:focus {
                background: $warning-light-color;
                border-color: $warning-light-color;
                color: #fff;
            }
            &:active {
                background: $warning-dark-color;
                border-color: $warning-dark-color;
                color: #fff;
                outline: none;
            }
            &.in-disabled {
                cursor: default;
                user-select: none;
                color: #f1f3f4;
                background-color: $warning-light-color;
                border-color: $warning-light-color;
                &:hover, &:focus {
                    background-color: $warning-light-color;
                }
                &:active {
                    border-color: $warning-light-color;
                }
                .icon {
                    fill: #f1f3f4;
                }
            }
        }
        &.in-circle {
            min-width: 31px;
            min-height: 30px;
            border-radius: 16px;
            padding: 15px 8px;
        }
        &.in-disabled {
            cursor: default;
            user-select: none;
            color: $border-color;
            &:hover, &:focus {
                background-color: $button-bg;
            }
            &:active {
                border-color: $border-color;
            }
            .icon {
                fill: $border-color;
            }
        }
        &.in-round {
            border-radius: 16px;
            padding: 15px 1em;
        }
    }
</style>