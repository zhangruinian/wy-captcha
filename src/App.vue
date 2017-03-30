<template>
    <div id="app">
        <h2>请把图片左上角红小方块拖动到缺角出，尽量保证准确性</h2>
        <ul>
            <li v-for="(img, index) in imgs" :key="index">
                <span @mousedown.prevent="down" @mousemove.prevent="move" @mouseup="up(index)"></span>
                <img :src="img" alt="">
                <a>确定</a>
            </li>
        </ul>
        <button @click="submit">提交</button>
    </div>
</template>

<script>
    import c1 from "./assets/imgs/1.jpg"
    import c2 from "./assets/imgs/2.jpg"
    import c3 from "./assets/imgs/3.jpg"
    import c4 from "./assets/imgs/4.jpg"


    export default {
        name: 'app',
        data(){
            return{
                select:null,
                isDown:false,
                ObjLeft:0,
                ObjTop:0,
                posX:0,
                posY:0,
                imgs:[
                    c1,c2,c3,c4
                ],
                positions:[],
                position:{}
            }
        },
        methods:{
            mousePosition(event){
                return{
                    x:event.pageX,
                    y:event.pageY
                }
            },
            down(event){
                this.isDown = true;
                this.ObjLeft = event.target.offsetLeft;
                this.ObjTop = event.target.offsetTop;
                this.posX = parseInt(this.mousePosition(event).x);
                this.posY = parseInt(this.mousePosition(event).y);
            },
            up(index){
                this.select = index;
                this.isDown = false;
               /* console.log(index);
                if ( this.select){
                    this.positions.splice(this.select,1)
                }*/
                this.positions.push(this.position);
            },
            move(event){
                if (this.isDown) {
                    var x = parseInt(parseInt(this.mousePosition(event).x) - this.posX + this.ObjLeft);
                    var y = parseInt(parseInt(this.mousePosition(event).y) - this.posY + this.ObjTop);
                    var w = 320 - event.target.clientWidth;
                    var h = 100 - event.target.clientHeight;
                    /*var w = document.documentElement.clientWidth - event.target.offsetWidth;
                    var h = document.documentElement.clientHeight - event.target.offsetHeight;*/
                    if (x < 0) {
                        x = 0
                    } else if (x > w) {

                        x = w
                    };
                    if (y < 0) {
                        y = 0
                    }else if (y > h) {
                        y= h
                    };
                    event.target.style.left = x + 'px';
                    event.target.style.top = y + 'px';
                    this.position ={
                        x:x,
                        y:y
                    }
                }
            },
            submit(){
                console.table(this.positions);
            }
        },
        components: {}
    }
</script>

<style scoped lang=less rel="stylesheet/less">
    #app {
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
        width: 100%;
        font-family: 'Avenir', Helvetica, Arial, sans-serif;
        color: #2c3e50;
        margin-top: 60px;
        ul{
            width: 800px;
            display: flex;
            flex-wrap: wrap;
            justify-content: space-between;
            li{
                list-style: none;
                margin-right: 20px;
                position: relative;
                span{
                    position: absolute;
                    display: inline-block;
                    width: 48px;
                    height: 38px;
                    /*background-color: red;*/
                    background-color: rgba(224,42,30,0.8);
                }
                a{
                    display: inline-block;
                    text-align: center;
                    width: 50px;
                    height: 30px;
                    line-height: 30px;
                    background-color: #000;
                    color: #fff;
                }
            }
        }
        button{
            width: 100px;
            height: 30px;
        }
    }
</style>
