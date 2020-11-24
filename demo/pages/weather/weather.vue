<template>
    <view class="container">
        <view class="weather-area" @click="setArea">{{ area }}</view>
        <image class="weather-img" :src="img"></image>
        <view class="weather-main">
            <text style="margin-right: 40rpx">{{ now.cond_txt }}</text
            ><text>{{ now.tmp }}°C</text>
        </view>
        <view class="weather-info">
            <view class="outside">
                <view class="inside">
                    <text style="color: #ddd; font-size: 30rpx">湿度</text>
                    <text>{{ now.hum }}%</text>
                </view>
                <view class="inside">
                    <text style="color: #ddd; font-size: 30rpx">气压</text>
                    <text>{{ now.pres / 10 }}kPa</text>
                </view>
                <view class="inside">
                    <text style="color: #ddd; font-size: 30rpx">能见度</text>
                    <text>{{ now.vis }}km</text>
                </view>
            </view>
            <view class="outside">
                <view class="inside">
                    <text style="color: #ddd; font-size: 30rpx">风向</text>
                    <text>{{ now.wind_dir }}</text>
                </view>
                <view class="inside">
                    <text style="color: #ddd; font-size: 30rpx">风速</text>
                    <text>{{ now.wind_spd }}km/h</text>
                </view>
                <view class="inside">
                    <text style="color: #ddd; font-size: 30rpx">风力等级</text>
                    <text>{{ now.wind_sc }}</text>
                </view>
            </view>
        </view>
        <u-picker
            mode="region"
            v-model="show"
            :params="params"
            @confirm="getArea"
            @cancel="cancelGetArea"
        ></u-picker>
    </view>
</template>

<script>
let location = "320113";
// key需要自行到和风天气官网注册并申请免费API接口的KEY!!
let key = "";
export default {
    data() {
        return {
            area: "江苏省-南京市-栖霞区",
            show: false,
            params: {
                province: true,
                city: true,
                area: true,
            },
            img: "../../static/weather/999.png",
            now: {
                cloud: "95",
                cond_code: "104",
                cond_txt: "阴",
                fl: "4",
                hum: "96",
                pcpn: "0.0",
                pres: "1024",
                tmp: "8",
                vis: "7",
                wind_deg: "45",
                wind_dir: "东北风",
                wind_sc: "4",
                wind_spd: "20",
            },
        };
    },
    onShow() {
        uni.request({
            url: "https://free-api.heweather.net/s6/weather/now?",
            method: "GET",
            data: {
                key,
                location,
            },
            success: (res) => {
                let now = res.data.HeWeather6[0].now;
                console.log(now);
                this.now = now;
                this.img = `../../static/weather/${now.cond_code}.png`;
            },
            fail: (reason) => {
                console.log(reason);
            },
        });
    },
    methods: {
        setArea() {
            this.show = true;
        },
        getArea(e) {
            this.area = `${e.province.label}-${e.city.label}-${e.area.label}`;
            location = e.area.value;
            uni.request({
                url: "https://free-api.heweather.net/s6/weather/now?",
                method: "GET",
                data: {
                    key,
                    location,
                },
                success: (res) => {
                    let now = res.data.HeWeather6[0].now;
                    console.log(now);
                    this.now = now;
                    this.img = `../../static/weather/${now.cond_code}.png`;
                },
                fail: (reason) => {
                    console.log(reason);
                },
            });
        },
        cancelGetArea() {
            this.show = false;
        },
    },
};
</script>

<style lang="scss">
@import "uview-ui/index.scss";

.container {
    font-family: Arial, Helvetica, sans-serif;
    background-image: linear-gradient(to top, #5ee7df 0%, #b490ca 100%);
    max-height: 100vh;
    min-height: 100vh;
    width: 100%;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: space-around;
}

.weather-area {
    margin-top: 60rpx;
    font-size: 45rpx;
    color: #fff;
}

.weather-main {
    display: flex;
    flex-direction: row;
    align-items: center;
    font-size: 60rpx;
    color: #fff;
}

.weather-img {
    width: 300rpx;
    height: 300rpx;
}

.weather-info {
    margin-top: 100rpx;
}

.outside {
    width: 100vw;
    height: 250rpx;
    display: flex;
    flex-direction: row;
}

.inside {
    flex: 1;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: space-evenly;
    font-size: 40rpx;
    line-height: 80rpx;
    color: #fff;
}
</style>
