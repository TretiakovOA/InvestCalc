<template>  
    <b-container>
        <h1>Расчет эффективности инвестиций</h1>        
        <b-form>           
            <b-card no-body class="mb-1">
                <b-card-header header-tag="header" class="p-1" role="tab">
                    <b-button block v-b-toggle.tab-1 variant="primary">Периоды расчета</b-button>
                </b-card-header>
                <b-collapse id="tab-1" role="tabpanel" visible>                    
                    <b-card-body>  
                        <b-card-text class="left">Выберите продолжительность одного периода:</b-card-text>                      
                        <b-form-select v-model="selectedTime" :options="times"></b-form-select>                        
                        <b-card-text class="left">Введите общее количество периодов:</b-card-text>
                        <b-form-input v-model="numPeriods" type="number" min="0" max="1000" @change="changePeriods(numPeriods)"></b-form-input>                         
                    </b-card-body>
                </b-collapse>
            </b-card>
            <b-card no-body class="mb-1">
                <b-card-header header-tag="header" class="p-1" role="tab">
                    <b-button block v-b-toggle.tab-2 variant="primary">Ставки</b-button>                    
                </b-card-header>
                <b-collapse id="tab-2" role="tabpanel">                    
                    <b-card-body>    
                        <b-card-text class="left">Введите годовую эффективную ставку дисконтирования, %:</b-card-text>                      
                        <b-form-input v-model="discountRate" type="number" min="0" max="100"></b-form-input>
                        <br/>
                        <b-form-checkbox id="check-reinvest" class="left" switch v-model="isReinvest">
                            Реинвестируются ли доходы?</b-form-checkbox>                         
                        <b-collapse :visible="isReinvest">
                            <b-card-text class="left">Введите годовую эффективную ставку реинвестирования, %:</b-card-text>
                            <b-form-input v-model="reinvestRate" type="number" min="0" max="100" :disabled="!isReinvest"></b-form-input>
                        </b-collapse>                                                                         
                    </b-card-body>
                </b-collapse>
            </b-card>
            <b-card no-body class="mb-1">
                <b-card-header header-tag="header" class="p-1" role="tab">
                    <b-button block v-b-toggle.tab-3 variant="primary">Потоки платежей</b-button>
                </b-card-header>
                <b-collapse id="tab-3" role="tabpanel">                    
                    <b-card-body>  
                       <!-- <b-card-text class="left">Введите дату начала проекта (<b>период, к которому относится дата, считается нулевым</b>):</b-card-text>
                       <b-form-input type="date" id="begin-date" v-model="zeroDate" @change="changeInDate(zeroDate)"></b-form-input>  -->
                       <!-- <label for="begin-date">{{ zeroDate }}</label>   -->
                       <!-- <b-collapse :visible="zeroDate != null"> -->                                                    
                        <b-card-text class="left">Введите потоки платежей по периодам (инвестиции и расходы вводить как отрицательные значения):</b-card-text>
                        <b-row v-for="period in periods" :key="period">                                
                            <b-col sm="3">
                                <label class="bottom" :for="`period-${period}`">Период {{ period }}:</label>
                            </b-col>
                            <b-col sm="9">
                                <b-form-input type="number" :id="`period-${period}`" v-model="cashflow[period]" ref="`period-${period}`"></b-form-input>
                            </b-col>
                        </b-row>                                            
                    </b-card-body>
                </b-collapse>
            </b-card>
            <br/>
            <b-button type="submit" variant="info" @click="onSubmit">Расчет</b-button>
            <b-button class="btn-margin" type="reset" variant="danger" @click="onReset">Сброс значений</b-button>
            <b-button class="btn-margin" variant="primary" @click="asFirst">Как в первом периоде</b-button>
            <br/>
            <b-alert v-model="showAlert" class="mt-3" dismissible>Расчет по приведенным данным невозможен!</b-alert>
            <br/>
            <b-card no-body class="mb-1" v-if="isResult">
                <b-card-header header-tag="header" class="p-1" role="tab">
                    <b-button block v-b-toggle.tab-1 variant="primary">Результаты расчета</b-button>
                </b-card-header>
                <b-collapse id="tab-1" role="tabpanel" visible>                    
                    <b-card-body>  
                        <b-card-text class="left">Чистый дисконтированный доход, NPV: {{npv}}</b-card-text>                      
                                                 
                    </b-card-body>
                </b-collapse>
            </b-card>
        </b-form>
    </b-container>
</template>
<script>
export default {
    data() {
        return {
            isReinvest: false,
            isResult: false, 
            showAlert: false,           
            numPeriods: 0,
            discountRate: 0,
            reinvestRate: 0,  
            npv: 0,
            selectedTime: 'year',
            //zeroDate: null,
            times: [                
                { value: 'year', text: 'год' },
                { value: 'halfyear', text: 'полугодие' },
                { value: 'quarter', text: 'квартал' },
                { value: 'month', text: 'месяц' }
            ],
            periods: ['0'],
            cashflow: [0],
        }
    },
    methods: {
        changePeriods(newValue) {
            console.log(newValue)            
            var len = this.periods.length            
            if (newValue >= len) {
                for(let i = len; i <= newValue; i++) {
                    this.periods.push(i.toString())
                    this.cashflow.push(0);
                }
            }
            else if (len > newValue) {
                for(let i = len - 1; i > newValue; i--) {
                    this.periods.pop()
                    this.cashflow.pop()
                }
            }            
        },
        asFirst() {            
            var len = this.cashflow.length
            if (len < 2) return
            var firstVal = this.cashflow[1]
            for(let i = len - 1; i > 1; i--) {
                this.periods.pop()
                this.cashflow.pop()
            }
            for(let i = 2; i < len; i++) {
                this.periods.push(i.toString())
                this.cashflow.push(firstVal);
            }
        },        
        onReset(evt) {
            evt.preventDefault()            
            this.cashflow = [0]
            this.periods = ['0']
            this.reinvestRate = 0 
            this.isReinvest = false
            this.discountRate = 0
            this.numPeriods = 0
            this.selectedTime = 'year'            
            this.isResult = false  
        },
        onSubmit(evt) {
            evt.preventDefault()
            if (this.numPeriods < 1 || this.discountRate <= 0 || this.cashflow[0] >= 0) {
                this.showAlert = true
                this.isResult = false
                return
            }
            this.npv = parseFloat(this.cashflow[0])
            for (let i = 1; i <= this.numPeriods; i++) {
                var discount = 1 / Math.pow(1 + (this.discountRate) / 100, i)
                this.npv += this.cashflow[i] * discount
            }
            this.showAlert = false
            this.isResult = true
            //action
        }
    }
}
</script>
<style>
    .left {
        margin-top: 10px;
        text-align: left;
    }
    .btn-margin {
        margin-left: 20px;
    }
    .bottom {
        padding-top: 10px;
    }    
</style>