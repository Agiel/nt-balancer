<template>
	<div class="hello">
		<span>Class: </span>
		<select v-model="currentClass">
			<option v-for="c in classes" :key="c">{{c}}</option>
		</select>
		<table>
			<tr>
				<th class="gun" @click="sortBy = 'name'">Gun</th>
				<th class="dmg" @click="sortBy = 'dmg'">Damage</th>
				<th class="ccl" @click="sortBy = 'cycle'">Cycle Time</th>
				<!-- <th>Penetration</th> -->
				<th class="hml" @click="sortBy = 'dmg'">Damage<br />(High / Mid / Low)</th>
				<th class="stk" @click="sortBy = 'dmg'">Shots To Kill<br />(High / Mid / Low)</th>
				<th class="ttk" @click="sortBy = 'ttk'">Time To Kill<br />(Mid)</th>
			</tr>
			<tr v-for="gun in gunTable" :key="gun.name">
				<td class="gun">{{gun.name}}</td>
				<td class="dmg"><input v-model.lazy="gunData[gun.id].damage"></td>
				<td class="ccl"><input v-model.lazy="gunData[gun.id].cycle"></td>
				<!-- <td>{{gun.penetration}}</td> -->
				<td class="hml">{{gun.hml}}</td>
				<td class="stk">{{gun.stk}}</td>
				<td class="ttk">{{gun.ttk}}</td>
			</tr>
		</table>
	</div>
</template>

<script>
import gunData from '../guns';

export default {
	name: 'HelloWorld',

	data() {
		return {
			gunData,
			currentClass: "Assault",
			classes: [
				"Recon",
				"Assault",
				"Support"
			],
			sortBy: "name"
		}
	},

	computed: {
		gunTable() {
			return this.gunData.map((gun, id) => {
				let modifier = {
					Recon: 1.12,
					Assault: 0.97,
					Support: 0.5
				};
				let dmg = gun.damage * modifier[this.currentClass];
				let bullets = gun.bullets || 1;

				let mid = (dmg * bullets).toFixed(0);
				let low = (dmg * 0.8 * bullets).toFixed(0);
				let high = (dmg * 1.35 * bullets).toFixed(0);

				let stkHigh = Math.ceil(100 / high);
				let stkMid = Math.ceil(100 / mid);
				let stkLow = Math.ceil(100 / low);

				return {
					id,
					name: gun.name,
					damage: gun.damage,
					cycle: gun.cycle,
					dmg: -gun.damage * bullets,
					hml:  high + " / " + mid + " / " + low,
					stk: stkHigh + " / " + stkMid + " / " + stkLow,
					ttk: Math.round((stkMid - 1) * gun.cycle * 1000),
				}
			}).sort((a, b) => a[this.sortBy] > b[this.sortBy] ? 1 : -1);
		}
	},
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
input {
	width: 80px;
}
.gun {
	text-align: left;
	width: 80px;
}
.dmg input, .ccl input {
	text-align: right;
}
.hml, .stk, .ttk {
	text-align: center;
}

.hml {
	width: 160px;
}

.stk {
	width: 160px;
}

.ttk {
	width: 120px;
}

th {
	cursor: pointer;
}

th:hover {
	background-color: #ddd;
}

tr:nth-child(even) {
	background-color: #eee;
}
</style>
