<template>
 <div id="popup" class="popup">
    <div class="popup__content">
      <div class="popup__body">
        <h3 class="popup__title">Налоговый вычет</h3>
        <p>Используйте налоговый вычет чтобы погасить ипотеку досрочно. Размер налогового вычета составляет не более 13%
          от своего официального годового дохода.</p>
        <Form @create='createSchedule' />
        <p v-if="payments.length" class="card">Итого можете внести в качестве досрочных:</p>
            <div v-for="(payment, idx) in payments" :key="idx" class="card__item">
              <input
                type="checkbox"
                name="checkbox"
                class="checkbox"
                :id="idx"
              />
              <label :for="idx" class="label">
                {{ formatter(payment) }} рублей &nbsp;
                <span> в {{ getOrdinalNum(idx + 1) }} год </span>
              </label>
            </div>
        <div class="choice">
          <div class="choice__text">Что уменьшаем?</div>
          <div class="buttons">
            <label>
              <input type="radio" name="radio" checked />
              <div class="payment box"><span>Платёж</span></div>
            </label>
            <label>
              <input type="radio" name="radio" />
              <div class="period box"><span>Срок</span></div>
            </label>
          
          </div>
        </div>
        <button class="btn btn-add">Добавить</button>
      </div>
     
      <button class="close" @click="$emit('close')">&times;</button>
    </div>

  </div>
</template>

<script>
import Form from './Form.vue'
export default {
  name: 'Card',
  data:()=> ({
    payments: ""
  }),
  components: {
    Form
  },
  methods: {
    createSchedule(data) {
      this.payments = data.payments
      console.log(data)
    },
    formatter(number) {
      return new Intl.NumberFormat('ru-RU').format(number)
    },
    getOrdinalNum(n) {
      console.log(n)
      return n+(1==n||4==n||5==n?'-ый':2==n||6==n||7==n||8==n?'-ой':'-ий')
    }
 
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.popup {
  /* display: none; */
  position: fixed;
  z-index: 1;
  left: 0;
  top: 0;
  height: 100%;
  width: 100%;
  overflow: auto;
  background: #BEC5CC;
  /* opacity: 0;
  visibility: hidden; */
}

.popup__content {
  margin: 120px auto;
  width: 552px;
  border-radius: 30px;
  background-color: #fff;
  padding: 2rem;
  animation-name: modalopen;
  animation-duration: var(--popup-duration);
  position: relative;
}

.popup__body {
  display: flex;
  flex-direction: column;
  align-items: start;
  justify-content: center;
} 

.popup__body h3 {
  font-size: 18px;
  font-weight: 500;
}

.popup__body p {
  color:  #808080;

}

input[type=number] {
  margin: 8px 0;
  padding: 8px 10px;
  width: 100%;
  border-radius: 3px;
  outline: none;
  border: 1px solid #DFE3E6;
}
input[type=number]:hover {
  border: 1px solid #000000;
}
input[type=number]:focus {
  border: 1px solid #DFE3E6;
}
input[type=number]:error {
  border: 1px solid #EA0029;
}

input[type=number].no-spinner::-webkit-inner-spin-button, 
input[type=number]::-webkit-outer-spin-button { 
-webkit-appearance: none; 
 margin: 0; 
}



.close {
position: absolute;
top: 22px;
right: 22px;
font-size: 22px;
padding: 0;
color: #EA0029;
border: none;
background: #fff;
}

.close:hover,
.close:focus {
  text-decoration: none;
  cursor: pointer;
}

.choice {
  display: flex;
  align-items: center;
  justify-content: center;
  margin-bottom: 40px;
}

.choice__text {
  margin-right: 2rem;
}

.buttons {
  display: flex;
}

input[type='radio']{
  display: none;
}

input[type="radio"]:checked + .box {
  background: var(--main-color);
  color: #fff;
}

.box {
  padding: 6px 12px;
  background-color: #EEF0F2;
  transition: all 250ms ease;
  /* will-change: transition; */
  display: inline-block;
  text-align: center;
  cursor: pointer;
  position: relative;
  border-radius: 50px;
}

.payment {
  margin-right: 10px;
}

.btn-add {
  width: 100%;
}

.card {
  color: #000000;
}
.checkbox {
  position: absolute;
  z-index: -1;
  opacity: 0;
}

.checkbox+label {
  display: inline-flex;
  align-items: center;
  user-select: none;
}
.checkbox+label::before {
  content: '';
  display: inline-block;
  width: 20px;
  height: 20px;
  flex-shrink: 0;
  flex-grow: 0;
  border: 1px solid #DFE3E6;
  /* border: 1px solid var(--main-color); */
  border-radius: 6px;
  margin-right: 12px;
  background-repeat: no-repeat;
  background-position: center center;
  background-size: 50% 50%;
}

.checkbox:checked+label::before {
  background: url("../assets/checked.svg") center center no-repeat, var(--main-color);
}

.checkbox:not(:checked)+label:hover::before {
  cursor: pointer;
}


.label {
  padding: 16px 0;
}

@keyframes modalopen {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}
</style>
