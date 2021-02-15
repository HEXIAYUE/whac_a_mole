<template>
    <div class="content">
        <header>
            <audio src="../assets/1.mp3" ref="audio"></audio>
            <div>
                <el-button type="primary" size="small" round @click="startGame" :disabled="isClick">开始游戏</el-button>
                <span><i class="el-icon-s-flag"></i>分数: {{count}}</span>
                <span><i class="el-icon-time"></i>倒计时: {{time}} S</span>
            </div>
        </header>
        <div class="row" v-for="y in size[1]" :key="y">
            <div class="box" v-for="x in size[0]" :key="x" @click.stop.prevent="hitGround(x,y)">
                <img v-show="isShow(x,y)" src="../assets/3.jpg" />
            </div>
        </div>
    </div>
</template>

<script>
import { Button } from 'element-ui';
export default {
    name: 'home',
    components: {
        elButton: Button
    },
    data () {
        return {
            size: [5, 4],
            mouseId: null, // 地鼠随机出现定时器的值
            site: [], // 地鼠出现的位置
            count: 0, // 分数
            time: 30, // 游戏倒计时 s
            timerId: null, // 倒计时定时器的值
            isClick: false // 是否能点击开始游戏按钮
        };
    },
    mounted () {
        // 适配移动端界面
        const ua = navigator.userAgent;
        const ipad = ua.match(/(iPad).*OS\s([\d_]+)/);
        const isIphone = !ipad && ua.match(/(iPhone\sOS)\s([\d_]+)/);
        const isAndroid = ua.match(/(Android)\s+([\d.]+)/);
        const isMobile = isIphone || isAndroid;
        if (isMobile) {
            this.size = [3, 3];
        }
    },
    methods: {
        // 初始化
        ready () {
            this.randomSite();
            this.mouseId = setInterval(this.randomSite, 1500);
        },
        // 渲染地鼠图片
        isShow (x, y) {
            return this.site[0] === x && this.site[1] === y;
        },
        // 地鼠位置随机产生
        randomSite () {
            const x = Math.floor(Math.random() * this.size[0] + 1);
            const y = Math.floor(Math.random() * this.size[1] + 1);
            this.site = [x, y];
        },
        // 游戏倒计时
        spendTime () {
            this.timerId = setInterval(() => {
                this.time--;
                if (this.time === 0) {
                    // 弹出游戏结束提示
                    this.$message({
                        message: '游戏结束',
                        type: 'success',
                        center: true,
                        duration: 1000
                    });
                    clearInterval(this.mouseId);
                    clearInterval(this.timerId);
                    // 地鼠位置设为空
                    this.site = [];
                    this.isClick = false;
                }
            }, 1000);
        },
        // 打击地鼠
        hitGround (x, y) {
            // 打中地鼠
            if (this.site[0] === x && this.site[1] === y) {
                // 产生音效
                this.$refs.audio.play();
                //  延迟下 使音效和动作能衔接好
                setTimeout(() => {
                    //  增加分数
                    this.count++;
                    // 重置地鼠位置和地鼠刷新时间
                    clearInterval(this.mouseId);
                    this.ready();
                }, 200);
            }
        },
        // 开始游戏
        startGame () {
            // 弹出游戏开始提示
            this.$message({
                message: '游戏开始',
                type: 'success',
                center: true,
                duration: 1000
            });
            this.time = 30;
            this.count = 0;
            this.ready();
            this.spendTime();
            this.isClick = true;
        }
    }
};
</script>

<style scope lang="scss">
.content {
    height: 400px;
    width: 450px;
    margin-left: 10%;

    header {
        width: 280px;
        margin-bottom: 40px;
        padding-left: 5px;

        > div {
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
    }

    .row {
        display: flex;
        .box {
            display: flex;
            justify-content: center;
            align-items: center;
            width: 80px;
            height: 80px;
            cursor: url('../assets/4.jpg'), auto;
            border: 5px solid #f9d67f;
            border-radius: 10px;
            background-image: url('../assets/2.jpg');

            img {
                width: 100%;
                height: 100%;
                object-fit: cover;
            }
        }
    }
}
</style>
