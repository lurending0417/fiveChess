<template>
    <div>
        <canvas id="FiveChessCanvas" width="450" height="450" @click="moveChess">
            Your browser does not support the Canvas API.
            Please upgrade your browser.
        </canvas>
    </div>
</template>
<script>
    class Chess {
        // 0 ===》 user
        // f ===》 computer
        constructor(ctx, color) {
            this.color = color;
            this.ctx = ctx;
        }

        moveStep(x, y) {
            let {color, ctx} = this
            ctx.beginPath();
            if (color === '0') {
                ctx.fillStyle = '#000';
            } else if (color === 'f') {
                ctx.fillStyle = '#c0eea4';
            }
            ctx.arc(x, y, 10, 0, 2 * Math.PI);
            ctx.fill();
            ctx.closePath();
        }
    }

    class Map {
        constructor(size, step) {
            this.size = size;
            let arr = [];
            for (let i = 0; i <= size; i++) {
                arr[i] = [];
                for (let j = 0; j <= size; j++) {
                    arr[i][j] = {
                        x: i, // x轴坐标
                        y: j * step, // y轴坐标
                        type: 'none' // 状态（white,black,none）
                    }
                }
            }
            this.arr = arr;
        }

        placeChess(i, j, type) {
            this.arr[i][j].type = type
        }

        hasChess() {
            let {arr, size} = this;
            let flag = false;
            for (let i = 0; i <= size; i++) {
                for (let j = 0; j <= size; j++) {
                    if (arr[i][j] === 1) {
                        flag = true;
                        break;
                    }
                }
            }
            return flag
        }

        isOver() {
            let {arr, size} = this;
            let flag = false;
            for (let i = 0; i <= size; i++) {
                for (let j = 0; j <= size; j++) {
                    if (arr[i][j] === 1) {
                        flag = true;
                        break;
                    }
                }
            }
            return flag
        }
    }

    export default {
        data() {
            return {
                ctx: null,
                step: 30,
                lenStep: 14,
                map: []
            }
        },
        methods: {
            DrawChessBoard() { // 绘制棋盘
                let {ctx, step, lenStep} = this;
                console.log('ctx', ctx)
                ctx.strokeStyle = '#ee5566';
                for (let i = 0; i <= lenStep; i++) {
                    // draw rows
                    ctx.moveTo(0, i * step)
                    ctx.lineTo(lenStep * step, i * step)
                    // draw cols
                    ctx.moveTo(i * step, 0)
                    ctx.lineTo(i * step, lenStep * step)
                }
                ctx.stroke();
                ctx.closePath();
                // 标注圆心
                ctx.beginPath();
                ctx.fillStyle = '#000';
                ctx.arc(lenStep / 2 * step, lenStep / 2 * step, 4, 0, 2 * Math.PI);
                ctx.fill();
            },
            moveChess(event) {
                let x = event.offsetX;
                let y = event.offsetY;
                new Chess(this.ctx, '0').moveStep(x, y)
            }
        },
        mounted() {
            this.ctx = document.getElementById('FiveChessCanvas').getContext('2d');
            this.DrawChessBoard()
            this.map = new Map(this.lenStep, this.step)
        }
    }
</script>