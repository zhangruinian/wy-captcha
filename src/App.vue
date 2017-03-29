<template>
    <div id="app">
        <h2>请把图片左上角红小方块拖动到缺角出，尽量保证准确性</h2>
        <ul>
            <li v-for="img in imgs">
                <span @mousedown.prevent="down" @mousemove.prevent="move" @mouseup="up"></span>
                <img :src="img" alt="">
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
            up(){
                this.isDown = false;
                this.positions.push(this.position)
            },
            move(event){
                if (this.isDown) {
                    var x = parseInt(parseInt(this.mousePosition(event).x) - this.posX + this.ObjLeft);
                    var y = parseInt(parseInt(this.mousePosition(event).y) - this.posY + this.ObjTop);
                    var w = document.documentElement.clientWidth - event.target.offsetWidth;
                    var h = document.documentElement.clientHeight - event.target.offsetHeight;
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
            width: 1000px;
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
            }
        }
        button{
            width: 100px;
            height: 30px;
        }
    }
</style>
