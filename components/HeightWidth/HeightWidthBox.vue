<style>
    .HeightWidth{position:relative;  }
    .HeightWidth-content{position:absolute;  top:0;  left:0;  height:100%;  width:100%;  }
</style>

<template>
    <div class="HeightWidth" :style="{ paddingTop:_paddingTop }">
        <div class="HeightWidth-content">
            <slot />
        </div>
    </div>
</template>

<script>
    export default{
        props : {
            hw:{
                type:Number,
                default:1,
            }
        },
        computed:{
            _paddingTop(){
                return this.hw*100+'%'
            }
        }
    }
</script>

