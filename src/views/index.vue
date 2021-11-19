<template>
	<div id="fx67ll-kb16" class="fx67ll-box">
		<div id="fx67ll-kb16-title" class="fx67ll-tip">☠️这是修炼的第{{ studentDays }}天，恭喜你距离结束仅剩😅</div>
		<div class="fx67ll-clock"></div>
	</div>
</template>

<script>
	import FlipClock from 'flipclock';
	import 'flipclock/dist/flipclock.css';
	import moment from 'moment';
	export default {
		name: 'fx67llIndex',
		data() {
			return {
				studentDays: 0,
				chnNumChar: ["零", "一", "二", "三", "四", "五", "六", "七", "八", "九"],
				chnUnitSection: ["", "万", "亿", "万亿", "亿亿"],
				chnUnitChar: ["", "十", "百", "千"]
			};
		},
		mounted() {
			this.initStuDays();
			this.initClock();
			this.listenWallpaper();
		},
		methods: {
			// 监听 wallpaper engine
			listenWallpaper() {
				window.wallpaperPropertyListener = {
					applyUserProperties: function(properties) {
						if (properties.customBgcolor) {
							// Convert the custom color to 0 - 255 range for CSS usage
							let customColor = properties.customBgcolor.value.split(' ');
							customColor = customColor.map(function(c) {
								return Math.ceil(c * 255);
							});
							let customColorAsCSS = 'rgb(' + customColor + ')';
							// Change background color
							document.getElementById('fx67ll-kb16').style.setProperty('background-color',
								customColorAsCSS);
						}
						if (properties.customTicolor) {
							// Convert the custom color to 0 - 255 range for CSS usage
							let customColor = properties.customTicolor.value.split(' ');
							customColor = customColor.map(function(c) {
								return Math.ceil(c * 255);
							});
							let customColorAsCSS = 'rgb(' + customColor + ')';
							// Change title color
							document.getElementById('fx67ll-kb16-title').style.setProperty('color',
								customColorAsCSS);
						}
					},
				};
			},
 			// 学习天数计算
			initStuDays() {
				let studentDays = moment(moment().format('YYYY-MM-DD')).diff(moment('2021-10-15').format('YYYY-MM-DD'),
					'day');
				this.studentDays = this.NumberToChinese(studentDays);
			},
			// 时钟初始化
			initClock() {
				const el = document.querySelector('.fx67ll-clock');
				const clock = new FlipClock(el, new Date(2022, 3, 31, 24, 0, 0, 0), {
					face: 'DayCounter', // 类型  
					showSeconds: true, // 显示秒数  
					showLabels: true, // 显示文字标识  
					language: {
						'years': '年',
						'months': '月',
						'days': '日',
						'hours': '時',
						'minutes': '分',
						'seconds': '秒'
					},
					countdown: true, // true为倒计时
				});
			},
			// 阿拉伯数字转换汉字数字
			NumberToChinese(num) {
				var unitPos = 0;
				var strIns = '',
					chnStr = '';
				var needZero = false;

				if (num === 0) {
					return this.chnNumChar[0];
				}

				while (num > 0) {
					var section = num % 10000;
					if (needZero) {
						chnStr = this.chnNumChar[0] + chnStr;
					}
					strIns = this.SectionToChinese(section);
					strIns += (section !== 0) ? this.chnUnitSection[unitPos] : this.chnUnitSection[0];
					chnStr = strIns + chnStr;
					needZero = (section < 1000) && (section > 0);
					num = Math.floor(num / 10000);
					unitPos++;
				}

				if (chnStr.substring(0, 1) === '一' && chnStr.substring(1, 2) === '十') {
					chnStr = chnStr.substr(1, chnStr.length)
				}

				return chnStr;
			},
			// 节内转换算法
			SectionToChinese(section) {
				var strIns = '',
					chnStr = '';
				var unitPos = 0;
				var zero = true;
				while (section > 0) {
					var v = section % 10;
					if (v === 0) {
						if (!zero) {
							zero = true;
							chnStr = this.chnNumChar[v] + chnStr;
						}
					} else {
						zero = false;
						strIns = this.chnNumChar[v];
						strIns += this.chnUnitChar[unitPos];
						chnStr = strIns + chnStr;
					}
					unitPos++;
					section = Math.floor(section / 10);
				}
				return chnStr;
			}
		}
	};
</script>
<style type="text/css">
	.flip-clock {
		font-size: 2vw;
		justify-content: center;
	}
</style>
<style lang="less" scoped="scoped">
	.fx67ll-box {
		width: 100%;
		height: 100%;
		overflow: hidden;
		background-color: #ffffff;
		.ban-user-select();

		.fx67ll-tip {
			width: 100%;
			text-align: center;
			font-size: 50px;
			font-weight: 900;
			color: #FF0000;
			position: absolute;
			top: 15%;
		}

		.fx67ll-clock {
			position: relative;
			top: 60%;
			margin-top: -10vw;
		}
	}
</style>
