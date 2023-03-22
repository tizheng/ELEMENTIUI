<template>
    <button 
        type="button"
        @click="change"
        class="a-button" 
        :disabled="disabled || loading || load" 
        :class="[theme, isBorder, isRound, sizes, blockCss]"
        :style="[minWidthCss]"
    >
        <span>
            <i v-if="loading || load" class="iconfont icon-prefix icon-loading"></i>
            <i v-if="iconPerfix" class="iconfont icon-prefix" :class="iconPerfix"></i>
            <slot />
            <i v-if="iconSuffix" class="iconfont icon-suffix" :class="iconSuffix"></i>
        </span>
    </button>
</template>

<script>
    export default {
        name: "AButton",
        props: {
            type: {
                type: String,
                default: ''
            },
            size: {
                type: String,
                default: ''
            },
            minWidth: {
                type: String,
                default: ''
            },
            perfix: {
                type: String,
                default: ''
            },
            suffix: {
                type: String,
                default: ''
            },
            loading: Boolean, // 默认false
            border: Boolean, // 默认false
            round: Boolean, // 默认false
            disabled: Boolean, // 默认false
            block: Boolean, // 默认false
            beforeChange: Function
        },
        computed: {
            theme(){
                return this.type ? `a-button-${this.type}` : ''
            },
            sizes(){
                return this.size ? `a-button-${this.size}` : ''
            },
            isBorder(){
                return this.border ? 'is-border' : ''
            },
            isRound(){
                return this.round ? 'is-round' : ''
            },
            minWidthCss(){
                if(!this.minWidth) { return "" }
                return { 'min-width': this.minWidth}
            },
            iconPerfix() {
                return this.perfix ? `icon-${this.perfix}` : ''
            },
            iconSuffix() {
                return this.suffix ? `icon-${this.suffix}` : ''
            },
            blockCss(){
                return this.block ? 'a-button-block' : ""
            }
        },
        data(){
            return {
                load: false
            }
        },
        methods: {
            // 事件回调
            change(){
                if(typeof this.beforeChange === 'function') {
                    this.load = true
                    this.beforeChange().then(response => {
                        this.load = false
                    }).catch(() => {
                        this.load = false
                    })
                }
                this.$emit('click')
            }
        }
    }
</script>

<style lang="scss" scoped>
.a-button {
    padding: 0 20px;
    border-width: 1px;
    border-style: solid;
    border-color: #dcdfe6;
    border-radius: 4px;
    background-color: #fff;
    font-size: 14px;
    color: #606266;
    height: 40px;
    cursor: pointer;
    + .a-button { margin-left: 14px; }
    > span {
        display: flex;
        align-items: center;
        justify-content: center;
    }
    .icon-prefix { margin-right: 10px; }
    .icon-suffix { margin-left: 10px; }
}
.a-button-medium {
    height: 36px;
}
.a-button-small {
    padding: 0 15px;
    height: 32px;
    font-size: 12px;
    .icon-prefix { margin-right: 5px; }
    .icon-suffix { margin-left: 5px; }
}
.a-button-mini {
    padding: 0 15px;
    height: 28px;
    font-size: 12px;
    .icon-prefix { margin-right: 5px; }
    .icon-suffix { margin-left: 5px; }
}
.a-button[disabled] {
    opacity: 0.5;
    cursor: not-allowed;
}
.a-button-primary {
    background-color: #409eff;
    border-color: #409eff;
    color: #fff;
    &.is-border {
        background-color: transparent;
        color: #409eff;
    }
}
.a-button-success {
    background-color: #00d100;
    border-color: #00d100;
    color: #fff;
    &.is-border {
        background-color: transparent;
        color: #00d100;
    }
}
.a-button-wraning {
    background-color: #e6a23c;
    border-color: #e6a23c;
    color: #fff;
    &.is-border {
        background-color: transparent;
        color: #e6a23c;
    }
}
.a-button-danger {
    background-color: #f56c6c;
    border-color: #f56c6c;
    color: #fff;
    &.is-border {
        background-color: transparent;
        color: #f56c6c;
    }
}
/** 圆角 */
.is-round { border-radius: 100px; }
/** 块级按钮 */
.a-button-block { display: block; width: 100%; }
/** 加载动画 */
.icon-loading {
    animation: loading 1s infinite linear;
}
@-webkit-keyframes loading {
    0% {
        -webkit-transform: rotate(0deg)
    }
    100% {
        -webkit-transform: rotate(360deg)
    }
}
</style>