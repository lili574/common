<template>
    <div id="input-img">
        <input @paste="handlePaste"
               readonly
               class="block-input"
               name="lili"
               :value="inputSrc"
               v-show="inputShow"
        >
        <div id="preview"
             :style="myShowClose ? pasteBorder : ''"
        >
            <img :src="inputSrc" v-show="myShowImg" alt="">
            <span class="span--close el-icon-close"
                  v-show="myShowClose"
                  @click="handleClose"
            >
            </span>
        </div>
    </div>
</template>

<script>
    export default {
        name: "InputImg",
        model: {
          prop: 'value',
          event: 'paste'
        },
        props: {
            value: {
              type: String,
              default: ''
          }
        },
        created(){
            this.inputSrc = this.value
        },
        beforemount(){
            this.inputShow = this.myShowImg ? false : true
        },
        data(){
            return {
                inputSrc: '',
                showImg: false,
                inputShow: true,
                closeShow: false,
                pasteBorder: {
                    'border': '1px solid #333',
                    'padding': '10px'
                }
            }
        },
        computed: {
            myShowImg(){
                return this.inputSrc.indexOf('png') === -1 && this.inputSrc.indexOf('jpg') === -1 ? false : true
            },
            myShowClose(){
                return this.myShowImg || this.closeShow ? true : false
            },
            myShowInput(){
                return this.myShowImg || this.inputShow ? true : false
            }
        },
        watch: {
            inputSrc(val, oVal) {
                console.log('val', val)
                console.log('oVal', oVal)
                this.$emit('paste', val)
            }
        },
        methods: {
            _checkImg(){
                let src = this.inputSrc ? this.inputSrc : ''
                if (!src) {
                    console.log('数据出错！')
                }
                let preview = document.querySelector('#preview')
                preview.innerHTML = `<img src="${src}">`;
                console.log('success')
            },
            _insertImg(event){
                const items = (event.clipboardData || window.clipboardData).items;
                let file = null;
                if (!items || items.length === 0) {
                    console.log("当前浏览器不支持本地");
                    return;
                }
                // 搜索剪切板items
                for (let i = 0; i < items.length; i++) {
                    if (items[i].type.indexOf("image") !== -1) {
                        file = items[i].getAsFile();
                        break;
                    }
                }
                if (!file) {
                    this.$message({
                        message: '粘贴内容非图片',
                    })
                    console.log("粘贴内容非图片");
                    return;
                }
                // console.log('nihao:::::::', this.$refs.container);
                // 此时file就是我们的剪切板中的图片对象

                const reader = new FileReader();
                reader.onload = event => {
                    this.inputSrc = event.target.result
                    this.closeShow = true
                    this.inputShow = false
                    this.$message({
                        message: '粘贴成功',
                        type: 'success'
                    })
                };
                // this.propSrc = event.target.value
                // console.log('paste event value++++++', this.outputSrc)
                // this.$emit('paste', this.outputSrc)
                reader.readAsDataURL(file);
            },
            handlePaste(event) {
                this._insertImg(event)
            },
            handleClose(){
                this.inputSrc = ''
                this.inputShow = true
                this.closeShow = false
            },
    }
}
</script>

<style lang="scss" scoped>
    #input-img {
        position: relative;
        margin-top: 10px;
        display: flex;
        .block-input {
            margin-right: 20px
        }
        #preview {
            display: inline-block;
            position: relative;
            font-size: 20px;
            img {
                box-sizing: border-box;
            }
            .span--close {
                position: absolute;
                top: 0;
                right: 0;
                transform: translate(50%, -50%);
                /*border: 2px solid #333;*/
                border-radius: 10px;
                opacity: .7;
                cursor: pointer;
                &:hover {
                    opacity: 1;
                }
            }
        }
    }
</style>
