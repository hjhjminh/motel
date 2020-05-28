<template>
    <div>
        <div>
            <h2 style="display: inline-block; margin-left: 33px">Thêm người</h2>
            <el-input v-model="numPeople" class="inp" size="medium" disabled></el-input>
            <button @click="decreasePerson" style="margin-left: 15px" class="iconBtn"><i class="el-icon-minus"></i>
            </button>
            <button @click="increasePerson" style="border-left: none" class="iconBtn"><i class="el-icon-plus"></i>
            </button>
        </div>


        <div v-if="numPeople > 0">
            <div style="display: inline-block; margin-left: 16px">
                <h2 style="display: inline-block">Tổng tiền nhà</h2>
                <el-input v-model="sumMoney" class="inp" size="medium"></el-input>
                <div style="display: inline-block; margin-left: 38px" v-if="countPeopleStayNotFullDay > 0">
                    <h2 style="display: inline-block">Tiền điện</h2>
                    <el-input class="inp" size="medium" v-model="electricBill"></el-input>
                    <h2 style="display: inline-block; margin-left: 32px">Tiền nước</h2>
                    <el-input class="inp" size="medium" v-model="waterBill"></el-input>
                </div>
            </div>

            <div>
                <h2 style="display: inline-block">Tên từng người</h2>
                <li style="display: inline-block" v-for="person in people" :key="person.id">
                    <el-input v-model="person.name" class="inp" size="medium" clearable></el-input>
                </li>
            </div>

            <div>
                <h2 style="display: inline-block; margin-left: 40px">Thời gian ở</h2>
                <li style="display: inline-block" v-for="person in people" :key="person.id">
                    <el-select v-model="person.timeStay" class="inp" v-on:change="change(person.timeStay)">
                        <el-option v-for="value in timeStays" :key="value" :label="value" :value="value">
                        </el-option>
                    </el-select>
                </li>
                <span style="margin-left: 15px">( Để tính tiền điện, nước)</span>
            </div>

            <div>
                <h2 style="display: inline-block; margin-left: -1px">Số tiền đã bỏ ra</h2>
                <li style="display: inline-block" v-for="person in people" :key="person.id">
                    <el-input v-model="person.moneySpent" class="inp" size="medium"></el-input>
                </li>
            </div>

            <div>
                <el-button style="margin-left: 175px; margin-top: 20px; width: 390.5px" type="primary"
                           @click="computedMoney" plain>Tính tiền
                </el-button>
            </div>

            <div v-if="computedClick">
                <ul v-for="person in people" :key="person.id" style="display: inline-block; color: blue">
                    <h2>{{person.name}}: {{person.lastMoney}}</h2>
                </ul>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        name: "ComputedMoney",
        data() {
            return {
                numPeople: 0,
                people: [],
                sumMoney: null,
                countPeopleStayNotFullDay: 0,
                timeStays: ['Full', '3 tuần', '2 tuần', '1 tuần', '0'],
                electricBill: null,
                waterBill: null,
                computedClick: false
            }
        },
        watch: {},
        methods: {
            computedMoney() {
                this.computedClick = true;
                const people = this.people;
                const length = people.length;
                let sumMoney = Number(this.sumMoney);
                people.forEach(person => (sumMoney += Number(person.moneySpent)));

                if (this.countPeopleStayNotFullDay > 0) {
                    const electricBill = Number(this.electricBill);
                    const waterBill = Number(this.waterBill);
                    const motelMoneyPerOne = (sumMoney - electricBill - waterBill) / length;
                    const electricWaterBillPerDay = (electricBill + waterBill) / (30 * length);

                    for (let i = 0; i < length; i++) {
                        const dayStay = this.getTimeStay(people[i].timeStay);
                        this.people[i].lastMoney = Math.round(motelMoneyPerOne + electricWaterBillPerDay * dayStay - people[i].moneySpent);
                    }
                } else {
                    const moneyPerOne = sumMoney / length;
                    for (let i = 0; i < length; i++) {
                        this.people[i].lastMoney = Math.round(moneyPerOne - people[i].moneySpent);
                    }
                }

            },
            getTimeStay(timeStay) {
                if (timeStay === this.timeStays[0]) {
                    return 30;
                } else if (timeStay === this.timeStays[1]) {
                    return 21;
                } else if (timeStay === this.timeStays[2]) {
                    return 14;
                } else if (timeStay === this.timeStays[3]) {
                    return 7;
                } else {
                    return 0;
                }
            },
            change(value) {
                if (value != 'Full') {
                    this.countPeopleStayNotFullDay++;
                } else {
                    this.countPeopleStayNotFullDay--;
                }
            },
            increasePerson() {
                if (this.computedClick) {
                    this.computedMoney();
                }
                this.numPeople += 1;
                let person = {
                    id: this.numPeople,
                    name: this.createNamePerson(),
                    moneySpent: null,
                    timeStay: 'Full',
                    lastMoney: 0
                }
                this.people.push(person);
            },
            decreasePerson() {
                if (this.computedClick) {
                    this.computedMoney();
                }
                if (this.numPeople > 0) {
                    this.numPeople -= 1;
                    this.people.pop();
                }
            },
            createNamePerson() {
                if (this.numPeople > 6) {
                    return "";
                } else if (this.numPeople % 6 === 1) {
                    return "Giang";
                } else if (this.numPeople % 6 === 2) {
                    return "Đức Anh";
                } else if (this.numPeople % 6 === 3) {
                    return "Trường";
                } else if (this.numPeople % 6 === 4) {
                    return "Mạnh";
                } else if (this.numPeople % 6 === 5) {
                    return "Phương";
                } else {
                    return "Khôi";
                }
            },
        }
    }
</script>

<style scoped>
    .iconBtn {
        background-color: white;
        border: 1px solid #DCDFE6;
        height: 36px;
        border-radius: 4px;
        width: 60px;
    }

    .inp {
        width: 120px;
        margin-left: 15px;
    }
</style>