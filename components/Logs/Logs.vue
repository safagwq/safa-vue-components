<style>
    .Logs{position:fixed; top:0; left:0; z-index:999999; background:rgba(255,255,255,0.5); color:#000; font-size:8px; white-space:pre; }
    .Logs{height:0;  width:0;  padding:0;  }
    .Logs-clear{position:fixed;  right:10px;  top:10px;  font-size:16px;  border:1px solid ;  padding:5px;  display:none;  }
    .Logs.show{height:50%; width:100%; padding:10px; }
    .Logs.show .Logs-clear{display:block;  }
    .toggleView{position:fixed;  height:30px;  width:30px;  background:#f00;  border-radius:50%;  opacity:0.5;  right:10px;  top:100px;  }
</style>

<template>

    <div :class="{ 'Logs':true, show:showLogs }" ref='Logs'>
        <div class='Logs-clear' @click='clear()'>清除</div>
        <div class='toggleView' @click='showLogs=!showLogs'></div>
        <div class='logs'>{{logs}}</div>
    </div>

</template>

<script>


    import Vue from 'vue'

    export default{
        data(){
            return{
                showLogs : false,
                logs : ''
            }
        },
        mounted : function(){
            Vue.prototype.$log=this.log
        },
        methods:{
            clear(){
                this.logs=''
            },
            log(value){

                console.log(value)

                if(typeof value=='object'){
                    value=JSON.stringify(value,null,2)
                }

                this.logs+=value+'\n\n'
            }
        },
    }
</script>