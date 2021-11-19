<template>
	<div id="fx67ll-kb16" class="fx67ll-box">
		<div id="fx67ll-kb16-title" class="fx67ll-tip">☠️这是学习的第{{ studentDays }}天，恭喜你距离毕业还有😅</div>
		<div class="fx67ll-clock"></div>
		<div class="fx67ll-footer">
			Designed & Powered by
			<a href="https://fx67ll.com" target="_blank">fx67ll</a>
			&#12288; Copyright© 2018-{{ this.year }}&#12288;
			<a href="https://beian.miit.gov.cn/#/Integrated/index" target="_blank">皖ICP备18017174号-2</a>
		</div>
		<el-dialog title="属性设置" :visible.sync="dialogVisible" width="200px">
			<div class="color-box">
				<div class="color-text">修改背景颜色：</div>
				<div class="color-item">
					<el-color-picker v-model="viewBgColor" @change="changeBgColor"></el-color-picker>
				</div>
			</div>
			<div class="color-box">
				<div class="color-text">修改标题颜色：</div>
				<div class="color-item">
					<el-color-picker v-model="viewTiColor" @change="changeTiColor"></el-color-picker>
				</div>
			</div>
			<span slot="footer" class="dialog-footer">
				<el-button type="primary" @click="closeDialog">确 定</el-button>
			</span>
		</el-dialog>
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
				chnUnitChar: ["", "十", "百", "千"],
				year: moment().format('YYYY'),
				dialogVisible: false,
				viewBgColor: '#ffffff',
				viewTiColor: '#FF0000'
			};
		},
		mounted() {
			this.initStuDays();
			this.initClock();
			this.consoleTips();
			this.consoleMyDays();
			this.consoleFailDays();
			this.listenKeys();
		},
		methods: {
			// 关闭面板
			closeDialog() {
				this.dialogVisible = false;
			},
			// 修改标题颜色
			changeTiColor() {
				document.getElementById('fx67ll-kb16-title').style.setProperty('color', this.viewTiColor);
			},
			// 修改背景颜色
			changeBgColor() {
				document.getElementById('fx67ll-kb16').style.setProperty('background-color', this.viewBgColor);
			},
			// 监听组合键 Ctrl + L
			listenKeys() {
				let self = this;
				document.onkeydown = function(event) {
					var e = event || window.event || arguments.callee.caller.arguments[0];
					if (e.ctrlKey && e.keyCode === 76) {
						self.dialogVisible = true;
					}
				};
			},
			// 提示
			consoleTips() {
				console.log('---------------Tips---------------');
				console.log('按下 Ctrl + L 可设置部分属性！');
				console.log('----------------------------------');
			},
			// 失败日
			consoleFailDays() {
				let failDays = moment(moment('20211113').format('YYYY-MM-DD')).diff(moment('20210823').format(
						'YYYY-MM-DD'),
					'day');
				console.log('fx67ll已于2021年11月13日获得失败，持续时间仅' + failDays + '天');
			},
			// 输出彩蛋
			consoleMyDays() {
				let myDays = moment(moment('2022-03-31').format('YYYY-MM-DD')).diff(moment().format('YYYY-MM-DD'),
					'day');
				console.log('fx67ll解决危机的时间仅剩：' + myDays + '天');
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
				const clock = new FlipClock(el, new Date(2022, 4, 31, 24, 0, 0, 0), {
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
		width: auto;
		font-size: 2vw;
		left: 6.5vw;
	}

	.flip-clock-single {
		left: 6.5vw;
	}

	.flip-clock-double {
		left: 6.5vw;
	}

	.flip-clock- {
		left: 6.5vw;
	}
</style>
<style lang="less" scoped="scoped">
	.fx67ll-box {
		width: 100%;
		height: 100%;
		overflow: hidden;
		color: #ffffff;
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

		.fx67ll-footer {
			width: 100%;
			padding: 20px 0;
			text-align: center;
			position: absolute;
			bottom: 0;
			user-select: text;
		}

		.color-box {
			width: 100%;
			display: flex;
			justify-content: space-between;
			align-items: center;

			.color-text {
				font-size: 16px;
			}
		}
	}
</style>
