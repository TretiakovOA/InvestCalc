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
                        <b-collapse>                            
                            <b-row v-for="period in periods" :key="period">                                
                                <b-col sm="3">
                                    <label :for="`period-${period}`">Период {{ period }}:</label>
                                </b-col>
                                <b-col sm="9">
                                    <b-form-input :id="`period-${period}`"></b-form-input>
                                </b-col>
                            </b-row>
                        </b-collapse>                    
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
            numPeriods: 0,
            discountRate: 0,
            reinvestRate: 0,  
            selectedTime: 'year',
            zeroDate: null,
            times: [                
                { value: 'year', text: 'год' },
                { value: 'halfyear', text: 'полугодие' },
                { value: 'quarter', text: 'квартал' },
                { value: 'month', text: 'месяц' }
            ],
            periods: ['0']
        }
    },
    methods: {
        changePeriods(newValue) {
            console.log(newValue)            
            var len = this.periods.length            
            if (newValue >= len) {
                for(let i = len; i <= newValue; i++) {
                    this.periods.push(i.toString())
                }
            }
            else if (len > newValue) {
                for(let i = len - 1; i > newValue; i--) {
                    this.periods.pop()
                }
            }            
        }
    }
}
</script>
<style>
    .left {
        margin-top: 10px;
        text-align: left;
    }
</style>