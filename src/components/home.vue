<template>
    <div class="content">
        <header>
            <audio src="../assets/1.mp3" ref="audio"></audio>
            <div>
                <div>
                    <el-button type="primary" size="small" round @click="startGame" :disabled="isClick">开始游戏</el-button>
                    <el-select v-model="level" placeholder="难度选择" @change="changelevel">
                        <el-option v-for="item in levelOptions" :key="item.value" :label="item.label"
                            :value="item.value">
                        </el-option>
                    </el-select>
                </div>
                <div>
                    <span><i class="el-icon-s-flag"></i>分数: {{count}}</span>
                    <span><i class="el-icon-time"></i>倒计时: {{time}} S</span>
                </div>
            </div>
        </header>
        <div class="row" v-for="y in size[1]" :key="y">
            <div class="box" v-for="x in size[0]" :key="x" @click.stop.prevent="hitGround(x,y)">
                <img v-show="isShow(x,y)" :src="imgSrc" />
            </div>
        </div>
    </div>
</template>

<script>
import { Button, Select } from 'element-ui';
export default {
    name: 'home',
    components: {
        elButton: Button,
        elSelect: Select
    },
    data () {
        return {
            size: [5, 4],
            mouseId: null, // 地鼠随机出现定时器的值
            site: [], // 地鼠出现的位置
            count: 0, // 分数
            time: 45, // 游戏倒计时 s
            timerId: null, // 倒计时定时器的值
            isClick: false, // 是否能点击开始游戏按钮
            level: null, // 难度等级
            levelOptions: [
                {
                    label: '低难度',
                    value: 0
                },
                {
                    label: '中难度',
                    value: 1
                },
                {
                    label: '高难度',
                    value: 2
                }
            ],
            speed: 1500, // 地鼠出现的速度
            // 地鼠图片
            imgList: [require('../assets/3.jpg'), require('../assets/5.jpg')],
            // 当前地鼠的图片
            imgSrc: null
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
            // 倒计时结束 不能调用函数
            if (this.time > 0) {
                this.imgSrc = this.imgList[0];
                this.randomSite();
                this.mouseId = setInterval(this.randomSite, this.speed);
            }
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
                    // 地鼠位置设为空
                    this.site = [];
                    clearInterval(this.mouseId);
                    clearInterval(this.timerId);
                    this.isClick = false;
                    // 弹出游戏结束提示
                    this.$message({
                        message: '游戏结束',
                        type: 'success',
                        center: true,
                        duration: 1000
                    });
                }
            }, 1000);
        },
        // 打击地鼠
        hitGround (x, y) {
            // 打中地鼠
            if (this.site[0] === x && this.site[1] === y && this.time > 0) {
                //  增加分数
                this.count++;
                // 切换为打中地鼠的图片
                this.imgSrc = this.imgList[1];
                console.log(this.imgSrc);
                // 产生音效
                this.$refs.audio.play();
                // 暂停循环
                clearInterval(this.mouseId);
                // 地鼠位置置为空 延迟为了展示被击中的动画
                setTimeout(() => {
                    this.site = [];
                }, 300);
                //  延迟下 使音效和动作能衔接好 随机时间出现下次地鼠
                setTimeout(() => {
                    // 重置地鼠位置和地鼠刷新时间
                    this.ready();
                }, (Math.random() + 1) * 1000);
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
            // 初始化时间和分数
            this.time = 45;
            this.count = 0;
            this.ready();
            this.spendTime();
            this.isClick = true;
        },
        // 改变游戏难度
        changelevel (val) {
            switch (val) {
            case 0:
                this.speed = 1500;
                break;
            case 1:
                this.speed = 1000;
                break;
            case 2:
                this.speed = 500;
                break;
            default:
                this.speed = 1500;
                break;
            }
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

        > div div {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-top: 5px;
        }
        .el-input__inner {
            width: 150px !important;
            height: 35px !important;
            line-height: 35px !important;
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
