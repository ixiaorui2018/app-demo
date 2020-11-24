<template>
    <view class="container">
        <view :style="bgImg" mode="cover" class="bgImg"></view>
        <view style="z-index: 10" class="music-info">
            <text>{{ title }}</text>
            <text>{{ singer }}</text>
        </view>
        <view class="music-box" style="z-index: 10">
            <view class="album-img" @click="bgPlay()">
                <image
                    :src="albumSrc"
                    class="romate"
                    mode="cover"
                    :style="playStatus"
                ></image>
            </view>
            <view class="control-box">
                <i
                    style="font-size: 60rpx; color: #ccc"
                    class="iconfont icon-shangyishou"
                    @click="lastSong()"
                ></i>
                <i
                    style="font-size: 60rpx; color: #ccc"
                    class="iconfont play"
                    :class="play"
                    @click="bgPlay()"
                ></i>
                <i
                    style="font-size: 60rpx; color: #ccc"
                    class="iconfont icon-xiayishou"
                    @click="nextSong()"
                ></i>
            </view>
        </view>
        <u-top-tips ref="uTips" style="z-index: 10"></u-top-tips>
    </view>
</template>

<script>
const innerAudioContext = uni.createInnerAudioContext();
innerAudioContext.loop = true;
let songs = [
    {
        title: "Something Just Like This",
        singer: "The Chainsmokers / Coldplay",
        coverImgUrl: "../../static/music/img/1.jpg",
        src: "../../static/music/mp3/1.mp3",
    },
    {
        title: "Counting Stars",
        singer: "OneRepublic",
        coverImgUrl: "../../static/music/img/2.jpg",
        src: "../../static/music/mp3/2.mp3",
    },
    {
        title: "Sound Of Silence",
        singer: "Simon & Garfunkel",
        coverImgUrl: "../../static/music/img/3.jpg",
        src: "../../static/music/mp3/3.mp3",
    },
    {
        title: "Valder Fields",
        singer: "Tamas Wells",
        coverImgUrl: "../../static/music/img/4.jpg",
        src: "../../static/music/mp3/4.mp3",
    },
];
let index = 0;
let isPlay = false;
innerAudioContext.src = songs[index].src;
export default {
    data() {
        return {
            bgImg: "background-image: url(../../static/music/img/1.jpg);",
            albumSrc: "../../static/music/img/1.jpg",
            playStatus: "animation-play-state: paused;",
            play: "icon-bofang",
            title: "Something Just Like This",
            singer: "The Chainsmokers / Coldplay",
        };
    },
    methods: {
        bgPlay() {
            if (isPlay) {
                innerAudioContext.pause();
                this.play = "icon-bofang";
                this.playStatus = "animation-play-state: paused;";
                this.$refs.uTips.show({
                    title: `Pause: ${songs[index].title}`,
                    duration: "1500",
                    type: "warning",
                });
            } else {
                innerAudioContext.play();
                this.play = "icon-zanting";
                this.playStatus = "animation-play-state: running;";
                this.$refs.uTips.show({
                    title: `Play: ${songs[index].title}`,
                    duration: "1500",
                    type: "success",
                });
            }
            isPlay = !isPlay;
        },
        lastSong() {
            if (index === 0) {
                this.$refs.uTips.show({
                    title: "The first song!",
                    duration: "1500",
                    type: "primary",
                });
            } else {
                index = index - 1;
                innerAudioContext.stop();
                innerAudioContext.src = songs[index].src;
                this.albumSrc = songs[index].coverImgUrl;
                this.title = songs[index].title;
                this.singer = songs[index].singer;
                innerAudioContext.play();
                isPlay = true;
                this.bgImg = `background-image: url(${songs[index].coverImgUrl});`;
                this.playStatus = "animation-play-state: running;";
                this.play = "icon-zanting";
                this.$refs.uTips.show({
                    title: `Play: ${songs[index].title}`,
                    duration: "1500",
                    type: "success",
                });
            }
        },
        nextSong() {
            if (index === songs.length - 1) {
                this.$refs.uTips.show({
                    title: "The last song!",
                    duration: "3500",
                    type: "primary",
                });
            } else {
                index = index + 1;
                innerAudioContext.stop();
                innerAudioContext.src = songs[index].src;
                this.albumSrc = songs[index].coverImgUrl;
                this.title = songs[index].title;
                this.singer = songs[index].singer;
                innerAudioContext.play();
                isPlay = true;
                this.bgImg = `background-image: url(${songs[index].coverImgUrl});`;
                this.playStatus = "animation-play-state: running;";
                this.play = "icon-zanting";
                this.$refs.uTips.show({
                    title: `Play: ${songs[index].title}`,
                    duration: "1500",
                    type: "success",
                });
            }
        },
    },
};
</script>

<style>
@import url("../../common/iconfont.css");

.container {
    font-family: Arial, Helvetica, sans-serif;
    background-image: linear-gradient(to top, #5ee7df 0%, #b490ca 100%);
    height: 100vh;
    width: 100%;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: space-around;
    position: relative;
}

.music-info {
    width: 100vw;
    padding: 10px 0;
    font-size: 32rpx;
    background-color: #aaaaaa;
    color: #ffffff;
    opacity: 0.8;
    line-height: 48rpx;
    display: flex;
    flex-direction: column;
    align-items: center;
}

.bgImg {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100vh;
    background-size: cover;
    filter: blur(5px);
}

.music-box {
    height: 160rpx;
    width: 680rpx;
    border-radius: 20rpx;
    background-color: #fff;
    box-shadow: 0 0 5px 1px #999;
    position: relative;
}

.play {
    transition-duration: 0.5s;
}

.romate {
    animation-name: imgRomate;
    animation-duration: 10s;
    animation-fill-mode: forwards;
    animation-iteration-count: infinite;
    animation-timing-function: linear;
}

@keyframes imgRomate {
    from {
        transform: rotate(0deg);
        transform-origin: center;
    }

    to {
        transform: rotate(360deg);
        transform-origin: center;
    }
}

.album-img {
    position: absolute;
    bottom: 30rpx;
    left: 20rpx;
    width: 200rpx;
    height: 200rpx;
    border-radius: 50%;
}

.album-img image {
    width: 200rpx;
    height: 200rpx;
    border-radius: 50%;
}

.control-box {
    width: 400rpx;
    line-height: 160rpx;
    margin-left: 240rpx;
    display: flex;
    flex-direction: row;
    justify-content: space-around;
}
</style>
