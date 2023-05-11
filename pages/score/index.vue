<template>
	<view class="container">
		<uni-section title="净资产" type="line" padding>
			<uni-easyinput v-model="asset.asset1" trim="all" @input="inputChange('asset')" placeholder="第一年净资产"></uni-easyinput>
			<uni-easyinput v-model="asset.asset2" trim="all" @input="inputChange('asset')" placeholder="第二年净资产"></uni-easyinput>
			<uni-easyinput v-model="asset.asset3" trim="all" @input="inputChange('asset')" placeholder="第三年净资产"></uni-easyinput>
		</uni-section>

		<uni-section title="销售收入" type="line" padding>
			<uni-easyinput v-model="income.income1" trim="all" @input="inputChange('income')" placeholder="第一年销售收入"></uni-easyinput>
			<uni-easyinput v-model="income.income2" trim="all" @input="inputChange('income')" placeholder="第二年销售收入"></uni-easyinput>
			<uni-easyinput v-model="income.income3" trim="all" @input="inputChange('income')" placeholder="第三年销售收入"></uni-easyinput>
		</uni-section>

		<uni-section title="财务指标" type="line" padding>
			<view class="" style="margin-bottom: 16px;">
				<uni-table border>
					<uni-tr>
						<uni-th align="center">近3年净资产增长率</uni-th>
						<uni-th align="center">得分</uni-th>
					</uni-tr>
					<uni-tr>
						<uni-td align="center">{{ assetRate ? assetRate : '0%' }}</uni-td>
						<uni-td align="center">{{ assetScore }}</uni-td>
					</uni-tr>
				</uni-table>
			</view>
			<view class="">
				<uni-table border>
					<uni-tr>
						<uni-th align="center">近3年销售收入增长率</uni-th>
						<uni-th align="center">得分</uni-th>
					</uni-tr>
					<uni-tr>
						<uni-td align="center">{{ incomeRate ? incomeRate : '0%' }}</uni-td>
						<uni-td align="center">{{ incomeScore }}</uni-td>
					</uni-tr>
				</uni-table>
			</view>
		</uni-section>

		<uni-section type="line" padding title="近三年财务综合得分" class="section">
			<template v-slot:right>
				<view class="score">{{ assetScore + incomeScore }}</view>
			</template>
		</uni-section>

		<button class="clear" type="default" size="mini" @click="clear">清空</button>
	</view>
</template>

<script>
export default {
	data() {
		return {
			asset: {
				// asset1: 44653758.05,
				// asset2: 45090611.66,
				// asset3: 45629438.9
				asset1: '',
				asset2: '',
				asset3: ''
			},
			assetRate: '0%',
			assetScore: 0,
			income: {
				// income1: 101105820.89,
				// income2: 133526614.04,
				// income3: 176414813.5
				income1: '',
				income2: '',
				income3: ''
			},
			incomeRate: '0%',
			incomeScore: 0
		}
	},
	methods: {
		inputChange(key) {
			const num1 = Number(this[key][`${key}1`])
			const num2 = Number(this[key][`${key}2`])
			const num3 = Number(this[key][`${key}3`])
			let rate = 0
			if (!this.isEmpty(this[key])) {
				if (num1 <= 0) {
					rate = Number(((num3 / num2) * 100).toFixed(2))
				} else {
					rate = Number((((num2 / num1 + num3 / num2 - 2) / 2) * 100).toFixed(2))
				}
			}
			if (num2 <= 0 || num3 < 0) {
				this[`${key}Score`] = 0
			} else {
				this[`${key}Score`] = this.getScore(rate)
			}
			this[`${key}Rate`] = rate + '%'
		},
		getScore(rate) {
			let score = 0
			// console.log(rate, 'rate')
			if (rate <= 0) {
				score = 0
			} else if (rate > 0 && rate < 5) {
				score = 1
			} else if (rate >= 5 && rate < 10) {
				score = 3
			} else if (rate >= 10 && rate < 15) {
				score = 4
			} else if (rate >= 15 && rate < 20) {
				score = 5
			} else if (rate >= 20 && rate < 25) {
				score = 6
			} else if (rate >= 25 && rate < 30) {
				score = 7
			} else if (rate >= 30 && rate < 35) {
				score = 8
			} else if (rate >= 35 && rate < 40) {
				score = 9
			} else if (rate >= 40) {
				score = 10
			}
			// console.log(score, 'getscore')
			return score
		},
		isEmpty(target) {
			for (let key in target) {
				if (!target[key]) {
					return true
				}
			}
			return false
		},
		clear() {
			this.income = {
				income1: '',
				income2: '',
				income3: ''
			}
			this.asset = {
				asset1: '',
				asset2: '',
				asset3: ''
			}
			this.assetRate = this.incomeRate = '0%'
			this.assetScore = this.incomeScore = 0
		}
	}
}
</script>

<style>
.container {
	line-height: 24px;
	position: relative;
}
.uni-easyinput {
	margin-bottom: 5px;
}
.score {
	display: flex;
	align-items: center;
	justify-content: center;
	width: 40px;
	height: 40px;
	border: 1px solid lightsteelblue;
	border-radius: 10px;
	color: firebrick;
	font-size: 20px;
	font-weight: bolder;
}
.clear {
	position: absolute;
	right: 10px;
	top: 10px;
}
</style>
