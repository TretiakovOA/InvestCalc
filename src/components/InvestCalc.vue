<template>    
    <b-container>
        <h1>Расчет эффективности инвестиций</h1>
        <b-form @submit="onSubmit" @reset="onReset">
            <b-card no-body class="mb-1">
                <b-card-header header-tag="header" class="p-1" role="tab">
                    <b-button block href="#" v-b-toggle.accordion-1 variant="primary">Периоды расчета</b-button>
                </b-card-header>
                <b-collapse id="accordion-1" role="tabpanel" visible="true">                    
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
                    <b-button block href="#" v-b-toggle.accordion-2 variant="primary">Ставки</b-button>
                </b-card-header>
                <b-collapse id="accordion-2" role="tabpanel">                    
                    <b-card-body>  
                        <b-card-text class="left">Введите ставку дисконтирования, %:</b-card-text>                      
                        <b-form-input v-model="discountRate" type="number" min="0" max="100"></b-form-input> 
                        <b-form-checkbox id="check-reinvest" v-model="isReinvest" class="left" 
                            switch>Реинвестируются ли доходы?</b-form-checkbox>                        
                        <b-card-text class="left">Введите ставку реинвестирования, %:</b-card-text>
                        <b-form-input v-model="reinvestRate" type="number" min="0" max="100"></b-form-input> 
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
            selectedTime: 'year',
            numPeriods: 0,
            discountRate: 0,
            reinvestRate: 0,
            isReinvest: false,
            times: [                
                { value: 'year', text: 'год' },
                { value: 'halfyear', text: 'полугодие' },
                { value: 'quarter', text: 'квартал' },
                { value: 'month', text: 'месяц' }
            ]
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