<template>    
    <b-container>
        <h1>Расчет эффективности инвестиций</h1>        
        <b-form>           
            <b-card no-body class="mb-1">
                <b-card-header header-tag="header" class="p-1" role="tab">
                    <b-button block v-b-toggle.accordion-1 variant="primary">Периоды расчета</b-button>
                </b-card-header>
                <b-collapse id="accordion-1" role="tabpanel" visible>                    
                    <b-card-body>  
                        <b-card-text class="left">Выберите продолжительность одного периода:</b-card-text>                      
                        <b-form-select v-model="selectedTime" :options="times"></b-form-select>                        
                        <b-card-text class="left">Введите общее количество периодов:</b-card-text>
                        <b-form-input v-model="numPeriods" type="number" min="0" max="1000"></b-form-input>                         
                    </b-card-body>
                </b-collapse>
            </b-card>
            <b-card no-body class="mb-1">
                <b-card-header header-tag="header" class="p-1" role="tab">
                    <b-button block v-b-toggle.accordion-2 variant="primary">Ставки</b-button>                    
                </b-card-header>
                <b-collapse id="accordion-2" role="tabpanel">                    
                    <b-card-body>    
                        <b-card-text class="left">Введите ставку дисконтирования, %:</b-card-text>                      
                        <b-form-input v-model="discountRate" type="number" min="0" max="100"></b-form-input>
                        <br/>
                        <b-form-checkbox id="check-reinvest" class="left" switch v-model="isReinvest">
                            Реинвестируются ли доходы?</b-form-checkbox> 
                        <!-- <label for="check-reinvest">{{ isReinvest }}</label> -->
                        <b-collapse :visible="isReinvest">
                            <b-card-text class="left">Введите ставку реинвестирования, %:</b-card-text>
                            <b-form-input v-model="reinvestRate" type="number" min="0" max="100" :disabled="!isReinvest"></b-form-input>
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
            selectedTime: 'year',
            numPeriods: 0,
            discountRate: 0,
            reinvestRate: 0,  
            times: [                
                { value: 'year', text: 'год' },
                { value: 'halfyear', text: 'полугодие' },
                { value: 'quarter', text: 'квартал' },
                { value: 'month', text: 'месяц' }
            ]
        }
    },
    methods: {
        toggleReinvest() {
            console.log('change reinvest')
            this.isReinvest = !this.isReinvest;
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