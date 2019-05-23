<style>
    .slid-toggle{overflow:hidden;  position:relative;  }

    .slid-toggle-view{position:absolute;  top:0;  left:0;  height:100%;  width:100%;  background:#fff;  }
    .slid-toggle-view-top{position:absolute;  bottom:100%;  left:0;  width:100%;  height:100%;  display:flex;  background:#fff;  flex-direction:column-reverse;  }
    .slid-toggle-view-bottom{position:absolute;  top:100%;  left:0;  width:100%;  height:100%;  display:flex;  background:#fff;  flex-direction:column;  }

    .slid-toggle-box{position:absolute;  height:100%;  width:100%;  top:-100%;  left:0;  transform:translateY(0%);  }
    .slid-toggle-item{height:100%;  width:100%;  position:relative;  }

</style>

<template>
    <div class='slid-toggle' v-if=' this.list.length!=0 ' v-slide='{ start , up , down , end }'>
        <div ref='slidToggleView' class='slid-toggle-view'>
            <div class='slid-toggle-box' ref='slidToggleBox'>
                <div class='slid-toggle-item'>
                    <slot :data=' __active.prev ' v-if='__active.prev' />
                </div>
                <div class='slid-toggle-item'>
                    <slot :data=' __active.now ' :isPlayer='true' />
                </div>
                <div class='slid-toggle-item'>
                    <slot :data=' __active.next ' v-if='__active.prev'  />
                </div>
            </div>

            <div class='slid-toggle-view-top'>{{ __viewTopText }}</div>
            <div class='slid-toggle-view-bottom'>{{ __viewBottomText }}</div>
        </div>
    </div>
</template>

<script>

    import Vue from 'vue'
    import Slide from '../js/Slide.js'

    export default{
        props : [],
        data : function (){
            return {
                index : 0,
                nowIndex : 0,
                list : [],

                viewTranslateY : 0,

                onloadState : 'init',
                isNotData : false,
            }
        },
        computed:{
            __viewTopText : function (){

                if(this.viewTranslateY>50){
                    return '释放刷新'
                }

                if(this.viewTranslateY==44 && this.onloadState=='loading'){
                    return '正在刷新...'
                }

                if(this.viewTranslateY>=0){

                    if(this.onloadState=='loaded'){
                        return '已刷新'
                    }

                    return '继续下拉刷新'
                }

                return ''
            },
            __viewBottomText : function (){

                if(this.viewTranslateY<-50){
                    return '释放刷新'
                }

                if(this.viewTranslateY==-44 && this.onloadState=='loading'){
                    return '正在刷新...'
                }

                if(this.viewTranslateY<=0){

                    if(this.onloadState=='loaded'){
                        return '已刷新'
                    }

                    return '继续上拉刷新'
                }

                return ''
            },
            __active : function (){

                return {
                    prev : this.list[ this.index-1 ],
                    now : this.list[ this.index ],
                    next : this.list[ this.index+1 ],
                }
            }
        },
        watch:{
            onloadState(newState){
                if(newState=='loaded'){
                    this.restoreView()
                }
            }
        },
        methods:{
            start: function (e,slideObj){
                
            },
            up: function (e,slideObj){
                var moveY=slideObj.touchData.changeY

                if(this.isNotData && this.index==this.list.length-1){
                    this.setViewTranslateY(moveY/2)
                    return
                }

                this.setTranslateY(moveY)
            },
            down: function (e,slideObj){
                var moveY=slideObj.touchData.changeY

                if(this.index==0){
                    this.setViewTranslateY(moveY/2)
                    return
                }

                this.setTranslateY(moveY)
            },
            end: function (e,slideObj){
                var touchData=slideObj.touchData

                if(!touchData.isMove){
                    return
                }

                if(touchData.firstDirection =='left' || touchData.firstDirection =='right'){
                    return
                }

                if(this.viewTranslateY){
                    this.restoreView()
                    return
                }

                if(touchData.firstDirection == touchData.lastViewDirection){
                    if(touchData.firstDirection=='down'){
                        if(this.index==0){
                            this.restore()
                        }
                        else{
                            this.toggle(this.index-1)
                        }
                    }
                    else{
                        this.toggle(this.index+1)
                    }
                }
                else{
                    this.restore()
                }
            },


            setViewTranslateY : function (moveY){
                this.viewTranslateY=moveY
                this.$refs.slidToggleView.style.transition = '0s'
                this.$refs.slidToggleView.style.transform = 'translateY('+moveY+'px)'
            },
            restoreView : function (){

                var c=this

                if(this.viewTranslateY>44){
                    this.viewTranslateY=44
                    this.onloadState='loading'
                    this.$emit('onInit')
                }
                else if(this.viewTranslateY<-44){
                    this.viewTranslateY=-44
                    this.onloadState='loading'
                    this.$emit('onInit')
                }
                else{
                    this.viewTranslateY=0
                    setTimeout(function(){
                        c.onloadState='init'
                    },300)
                }

                this.$refs.slidToggleView.style.transform = 'translateY('+this.viewTranslateY+'px)'
                this.$refs.slidToggleView.style.transition = '0.4s'
            },

            setTranslateY : function (moveY){

                this.$refs.slidToggleBox.style.transition = '0s'
                this.$refs.slidToggleBox.style.transform = 'translateY('+moveY+'px)'
            },
            restore : function (){
                this.$refs.slidToggleBox.style.transform = 'translateY(0px)'
                this.$refs.slidToggleBox.style.transition = '0.4s'
            },


            toggle : function (nowIndex){

                this.nowIndex=nowIndex

                if(nowIndex > this.index){
                    this.$refs.slidToggleBox.style.transform = 'translateY(-100%)'
                }
                else{
                    this.$refs.slidToggleBox.style.transform = 'translateY(100%)'
                }

                this.$refs.slidToggleBox.style.transition = '0.4s'
            },
            toggleEnd : function (){

                this.index=this.nowIndex
                this.$emit('onChange' , this.index)

                if( this.index>=this.list.length-2 ){
                    this.$emit('onLoadMore')
                }

                this.setTranslateY(0)

                // if(this.nowIndex == this.index)return

                // var item2=this.$refs.slidToggleBox.children[1]
                // var item3=this.$refs.slidToggleBox.children[2]


                // if(this.nowIndex<this.index){


                //     // console.log('<<')
                // }
                // else{
                //     // this.$refs.slidToggleBox.
                //     // console.log('>>')
                // }
            },


            init : function (list){
                this.list = list.map(a=>a)
                this.isNotData = false
                this.index=0
                this.nowIndex=0

                if(this.onloadState == 'loading'){
                    this.onloadState = 'loaded'
                }
                else{
                    this.onloadState = 'init'
                }

                
            },
            append : function (data){
                this.list.push(data)
            },
            notData : function (data){
                this.isNotData=true
            }
        },

        mounted : function(){
            var c=this

            var interval=setInterval(function (){
                if(!c.$refs.slidToggleBox)return

                clearInterval(interval)
                c.$refs.slidToggleBox.addEventListener('transitionend',function(e){
                    c.toggleEnd()
                })
            },100)
        },
    }
</script>