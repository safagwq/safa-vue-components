<style>
    .Keyboard{position:fixed;  top:0;  left:0;  height:100%;  width:100%;  z-index:9999;  background:rgba(0,0,0,0.3);  user-select:none;  opacity:0;  pointer-events:none;  transform:translateY(-120vh);  }
    .Keyboard-bg{position:absolute;  top:0;  left:0;  right:0;  bottom:0;  }
    .Keyboard,
    .Keyboard *{box-sizing:border-box;  }
    .Keyboard-top{position:absolute;  top:15vw;  margin:auto;  width:300px;  left:0;  right:0;  overflow:hidden;  border-radius:5px;  background:#fff;  padding:10px;  opacity:0;  }
    .Keyboard-input{display:flex;  font-size:20px;  border:1px solid #ccc;  border-radius:5px;  height:40px;  }
    .Keyboard-grid{flex:1;  display:flex;  justify-content:center;  align-items:center;  border-right:1px solid #ccc;  position:relative;  }        

    .Keyboard-body{position:absolute;  bottom:0;  width:100%;  left:0;  padding-bottom:60%;  background:#ddd;  font-size:24px;  opacity:0;  }
    .Keyboard-body-box{position:absolute;  top:0;  left:0;  height:100%;  width:100%;  display:flex;  flex-direction:column;  padding:3px;  }
    .Keyboard-functions{position:absolute;  background:#eee;  bottom:100%;  left:0;  right:0;  display:flex;  flex-direction:row-reverse;  }
    .Keyboard-function{color:#4277c1;  padding:10px;  font-size:14px;  }

    /* 输入值 */
    .Keyboard-grid.hasValue::after{content:attr(data-value);  background:none;  height:auto;  width:auto;  }

    /* 光标 */
    .Keyboard-grid.active::after{opacity:0.2;  }
    .Keyboard-grid.active::before{content:'';  height:1em;  width:2px;  background:#666;  animation:Keyboard-grid-blink 1s infinite;  flex-shrink:0;  }

    .Keyboard-grid:last-child{border-right:none;  }
    .Keyboard-title{text-align:center;  font-size:16px;  line-height:40px;  margin-top:-10px;  }
    .Keyboard-title:empty{display:none;  }

    .Keyboard-close{position:absolute;  left:0;  top:0;  height:40px;  width:40px;  }

    .Keyboard-line{display:flex;  flex:1;  }
    .Keyboard-key{display:flex;  flex:1;  padding:3px;  }
    .Keyboard-key[data-key-value='none']{flex:0.5;  opacity:0;  pointer-events:none;  }
    .Keyboard-key[data-key-value='space']{flex:2;}

    .Keyboard-key-target{display:flex;  flex:1;  border-radius:5px;  justify-content:center;  align-items:center;  height:100%;  background:#fff;  }
    .Keyboard-key-target::before{content:attr(data-key-value);  }
    .Keyboard-key-target[data-key-value='none']{display:none;  }
    .Keyboard-key-target[data-key-value='hide']{opacity:0;  pointer-events:none;  }

    .Keyboard-key-target[data-key-value='shift']{background:#fff url(./icon/shift.png) center/auto 24px   no-repeat;  }
    .Keyboard-key-target[data-key-value='del']{background:#fff url(./icon/del.png) center/auto 22px   no-repeat;  }
    .Keyboard-key-target[data-key-value='end']{background:#fff url(./icon/enter.png) center/auto 24px   no-repeat;  }
    .Keyboard-key-target[data-key-value='symbol']{background:#fff url(./icon/language.png) center/auto 24px   no-repeat;  }

    .Keyboard-key-target[data-key-value='shift']::before,
    .Keyboard-key-target[data-key-value='del']::before,
    .Keyboard-key-target[data-key-value='end']::before,
    .Keyboard-key-target[data-key-value='symbol']::before,
    .Keyboard-key-target[data-key-value='space']::before{content:' ';  }

    

    /* 自由模式 */
    .Keyboard.infiniteModel .Keyboard-input{padding:0 5px;  }
    .Keyboard.infiniteModel .Keyboard-grid{flex:initial;  width:auto;  border-right:none;  }
    .Keyboard.infiniteModel .Keyboard-grid.active::after{opacity:1;  }
    .Keyboard.infiniteModel [data-value=" "]{flex:1;  display:flex;  }
    .Keyboard.infiniteModel [data-value=" "]::after{content:'';  flex:1;  }
    .Keyboard.infiniteModel [data-value=" "]:not(:last-child){flex:initial;  width:5px;  }

    /* 密码模式 */
    .Keyboard.passwordModel .Keyboard-grid.hasValue{padding:2px;  }
    .Keyboard.passwordModel .Keyboard-grid.hasValue::after{content:'';  height:5px;  width:5px;  border-radius:50%;  background:currentColor;  display:block;  }

    /* 激活shift */
    .Keyboard.shiftActive .Keyboard-key-target[data-key-value='shift']{background-color:#bbb;  }
    .Keyboard.shiftActive .Keyboard-key-target{text-transform:uppercase;  }

    /* 禁用某些按钮 */
    .Keyboard.disabledPoint .Keyboard-key[data-key-value='.'],
    .Keyboard.disabled_0 .Keyboard-key[data-key-value='0'],
    .Keyboard.disabledDel .Keyboard-key[data-key-value='del']{opacity:0.6;  pointer-events:none;  color:#999;  }


    /* 动画 */
    .Keyboard{animation:Keyboard-show 0.3s forwards;  pointer-events:auto;  }
    .Keyboard .Keyboard-top{animation:Keyboard-top-show 0.2s ease 0.1s forwards;  }
    .Keyboard .Keyboard-body{animation:Keyboard-body-show 0.3s ease 0.2s forwards;  }

    .Keyboard.hide{animation:Keyboard-hide 0.6s forwards;  }
    .Keyboard.hide .Keyboard-top{animation:Keyboard-top-hide 0.2s forwards;  }
    .Keyboard.hide .Keyboard-body{animation:Keyboard-body-hide 0.3s forwards;  }

    @keyframes Keyboard-hide{
        100%{opacity:0;  transform:translateY(-120vh);  }
        99%{opacity:0;  transform:translateY(0);  }
        50%{opacity:1;  transform:translateY(0);  }
        0%{opacity:1;  transform:translateY(0);  }
    }

    @keyframes Keyboard-top-hide{
        0%{transform:translateY(0px);  opacity:1;  }
        100%{transform:translateY(-50px);  opacity:0;  }
    }

    @keyframes Keyboard-body-hide{
        0%{transform:translateY(0px);  opacity:1;  }
        30%{transform:translateY(0px);  opacity:1;  }
        100%{transform:translateY(50px);  opacity:0;  }
    }

    @keyframes Keyboard-show{
        0%{opacity:0;  transform:translateY(-120vh);  }
        1%{opacity:0;  transform:translateY(0);  }
        100%{opacity:1;  transform:translateY(0);  }
    }

    @keyframes Keyboard-top-show{
        0%{transform:translateY(-50px);  opacity:0;  }
        100%{transform:translateY(0px);  opacity:1;  }
    }

    @keyframes Keyboard-body-show{
        0%{transform:translateY(50px);  opacity:0;  }
        100%{transform:translateY(0px);  opacity:1;  }
    }

    @keyframes Keyboard-grid-blink{
        0%{opacity:1;  }
        30%{opacity:1;  }
        50%{opacity:0.1;  }
        80%{opacity:0.1;  }
        100%{opacity:1;  }
    }

</style>

<template>
    <div v-if='view.selfShow' :class="{
        'Keyboard':true,
        hide: !show, 
        infiniteModel, 
        passwordModel,
        shiftActive : view.shiftActive,

        disabledPoint : _isDisabledPoint,
        disabled_0 : _isDisabled_0,
        disabledDel : _isDisabledDel
    }">
        <div class='Keyboard-bg' @click=" blur() "></div>

        <div class="Keyboard-top" v-if=" !hideTop ">
            <div class="Keyboard-title">{{title}}</div>
            <div class="Keyboard-input">
                <span v-for="(text,index) in valueArr" :key='index' @click="setActiveIndex(index)" :class="{ 'Keyboard-grid':true , 'hasValue' : text!=' ' , active:index==activeIndex }" :data-value='text'></span>
            </div>
        </div>

        <div class="Keyboard-body">

            <div class="Keyboard-functions" v-if="hideFunctions!='true'">
                <div class="Keyboard-function" @click='end'>完成</div>
                <div class="Keyboard-function" @click='blur'>取消</div>
            </div>

            <div class="Keyboard-body-box">
                <div class="Keyboard-line" v-for="(keyLine,lineIndex) in _values" :key='lineIndex'>
                    <div class="Keyboard-key" :data-key-value="keyValue" v-for="(keyValue,valueIndex) in keyLine" :key='valueIndex' @click="pushValue(keyValue)">
                        <div class="Keyboard-key-target" :data-key-value="keyValue"></div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    export default{

        props : {
            show : {
                type: Boolean,
                required: true
            },
            value : {
                type: String,
                required: true
            },
            title : {
                type: String,
                defualt: '',
            },
            type : {
                type: String,
                defualt: 'number',
            },
            length : {
                type: Number,
                defualt: 0,
            },
            maxLength : {
                type: Number,
                defualt: 20,
            },
            passwordModel : {
                type : Boolean,
                defualt : false,
            },
            hideFunctions : {
                type : Boolean,
                defualt : false,
            },
            hideTop : {
                type : Boolean,
                defualt : false,
            },
            pointAfterLength : {
                type : Number,
                defualt : 0,
            }
        },


        data : function (){

            var valueLength=this.length
            var valueMaxLength=this.maxLength

            return {
                view : {
                    shiftActive : false,
                    selfShow : this.show,
                    showSymbol : false,
                },
                keys : {
                    number : [
                        '123'.split(''),
                        '456'.split(''),
                        '789'.split(''),
                        '.,0,del'.split(','),
                    ],
                    tel : [
                        '123'.split(''),
                        '456'.split(''),
                        '789'.split(''),
                        'hide,0,del'.split(','),
                    ],
                    text : [
                        '1234567890'.split(''),
                        'qwertyuiop'.split(''),
                        'none,a,s,d,f,g,h,j,k,l,none'.split(','),
                        'shift,z,x,c,v,b,n,m,del'.split(','),
                        'symbol,space,.,end'.split(','),
                    ],
                    symbol : [
                        '<>[]{}()/\\'.split(''),
                        '+-*/~=%?!|'.split(''),
                        `:;'"^¥&@#•`.split(''),
                        `none \` - _ … $ € £ del none`.split(' '),
                        'symbol,space,.,end'.split(','),
                    ],
                },

                functionKeys : ['shift','del','end','space','symbol'],

                closeTimeOut : null,
                infiniteModel : !valueLength,
                disabledInput : false,
                numberPointAfterLength : this.pointAfterLength,
                valueLength : valueLength,
                valueMaxLength : valueMaxLength,
                valueArr : [],
                activeIndex : 0,
                
            }
        },



        watch:{
            value : function (newValue,oldValue){
                if(newValue == this._valueStr){
                    return
                }

                this.init(newValue)
            },
            show : function (newValue){
                if(newValue==true){
                    clearTimeout(this.closeTimeOut)
                    this.view.selfShow = true
                }
                else{
                    this.closeTimeOut = setTimeout(()=>{
                        this.view.selfShow = false
                    },500)
                }
            }
        },



        computed:{
            _values(){
                if(this.view.showSymbol){
                    return this.keys.symbol
                }

                return this.keys[this.type]
            },
            _valueStr : function (){
                return this.valueArr.filter(function (char){ return char!=' ' }).join('')
            },

            _isDisabledPoint : function (){
                return this.passwordModel!='true' && this.infiniteModel && this.valueArr.indexOf(".")!=-1
            },

            _isDisabled_0 : function (){
                var disabled_0 = this.passwordModel!='true' && this.valueArr.slice(0,this.activeIndex).every(function (num){
                    return num=='0'
                })

                if(this.valueArr[0]==' '){
                    return false
                }
                return disabled_0
            },
            _isDisabledDel : function (){
                return false
            }
        },

        mounted : function(){
            this.init(this.value)
        },
        methods:{

            init(newValue){

                var newValueParse = parseFloat(newValue)

                if(isNaN(newValueParse) && newValue!=''){
                    return
                }

                var newValueArr=( newValue==''?newValue:newValueParse ).toString().split('')

                if(this.infiniteModel){
                    newValueArr.push(' ')
                    this.activeIndex = newValueArr.length -1
                }
                else{
                    if(newValueArr.length > this.valueLength){
                        newValueArr.length = this.valueLength
                    }
                    else{
                        for(var i=newValueArr.length;i<this.valueLength;i++){
                            newValueArr.push(' ')
                        }
                    }

                    var activeIndex=newValueArr.indexOf(' ')
                    if(activeIndex==-1){
                        activeIndex=newValueArr.length
                    }

                    this.activeIndex = activeIndex
                }


                if(newValueArr.length>=this.valueMaxLength){
                    newValueArr.length=this.valueMaxLength
                    return
                }

                this.valueArr = newValueArr
            },

            pushValue : function (value){
                if(this.functionKeys.includes(value)){
                    this[value]()
                    return
                }

                if(this.disabledInput){
                    return
                }

                if(this.valueArr.length==this.valueMaxLength){
                    return
                }

                if(this.view.shiftActive){
                    value=value.toUpperCase()
                }

                if(this.infiniteModel){
                    if(this.numberPointAfterLength){
                        var pointIndex = this.valueArr.indexOf('.')
                        if(pointIndex!=-1 && this.valueArr.length - pointIndex - 1 > this.numberPointAfterLength){
                            return
                        }
                    }
                    this.valueArr.splice(this.activeIndex,0,value)
                }
                else{
                    if(this.valueArr.length == this.activeIndex){
                        return
                    }
                    this.$set(this.valueArr , this.activeIndex , value)
                }


                if(value=='.' && this.activeIndex==0 && this.passwordModel!='true'){
                    this.valueArr.unshift('0')
                    this.activeIndex++
                }

                this.activeIndex++ 

                this.$emit('input',this._valueStr)
            },
            setActiveIndex : function (index){
                if(this.valueArr[index-1]==' '){
                    index = this.valueArr.indexOf(' ')
                }

                this.activeIndex=index
            },
            symbol(){
                this.view.showSymbol=!this.view.showSymbol
            },
            space(){
                this.pushValue(' ')
            },
            shift(){
                this.view.shiftActive=!this.view.shiftActive
            },
            del(){

                this.activeIndex--
                if(this.activeIndex<0){
                    this.activeIndex=0
                    if(this.valueArr[0]==' '){
                        return
                    }
                }

                if(this.infiniteModel){
                    this.valueArr.splice(this.activeIndex,1)
                }
                else{
                    this.valueArr.splice(this.activeIndex,1)
                    this.valueArr.push(' ')
                }

                this.$emit('input',this._valueStr)
            },
            end : function (){
                this.$emit('end',this._valueStr)
                this.$emit('update:show',false)
            },
            blur : function (){
                this.$emit('blur',this._valueStr)
                this.$emit('update:show',false)
            },
        }
    }
</script>