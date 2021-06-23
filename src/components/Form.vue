<template>
  <form
    @submit.prevent="createPayments()"
  >
    <label>Ваша зарплата в месяц</label>
    <input class="no-spinner" v-model.lazy="v$.form.salary.$model"
            placeholder="Введите данные"
            @blur="v$.form.salary.$touch"
            type="number"
            v-focus
            >

    <div v-if="v$.form.salary.$errors&&v$.form.salary.$dirty">
      <span class="error" v-if="v$.form.salary.required.$invalid">
        Поле обязательно для заполнения
      </span>
      <span class="error" v-if="v$.form.salary.minValue.$invalid">
        Некорректные данные
      </span>
    </div>
    <button class="count" type="submit" >Рассчитать</button>
        <!-- <div class="count">Рассчитать</div> -->
  </form>

</template>

<script>
import useVuelidate from '@vuelidate/core'
import { required, minValue} from '@vuelidate/validators'

export default {
  name: "Form",
  setup () {
    return { 
      v$: useVuelidate() 
    }
  },
  // setup: () => ({

  // }),
  data() {
    return {
      form: {
        salary: ""
        },
      payments: [],

    };
  },
  validations() {
    return {
      form: {
      salary: {
        required,
        minValue: minValue(1000),
      }
    },
  }
    
  },
  methods: {
    createPayments(){
      // this.payments=[]
      const MAX_PRICE = 2000000
      let flatPrice = 2600000
  
      const MONTHS = 12
      const TAX_AMOUNT = 0.13
      const annualTax = this.form.salary * MONTHS * TAX_AMOUNT
      const maxTax = flatPrice < MAX_PRICE ? flatPrice * TAX_AMOUNT : 260000
      const yearsToPay = Math.ceil(maxTax / annualTax)
      this.v$.$touch()
      if (this.v$.$error) return

      for (let i = 1; i <= yearsToPay; i++) {
        if (i == yearsToPay) {
          let payment = maxTax % annualTax
          this.payments.push(payment)
          console.log(payment)
        } else {
          this.payments.push(annualTax)
        }
      }
      console.log(this.payments)
      this.create()
      console.log(this.payments)
    },
    create(){
      this.$emit('create', {payments: this.payments });
       this.payments = []
    }
  },
  directives: {
    focus: {
      mounted(el) {
        el.focus()
      }
    }
  } 
}
</script>

<style>
form {
  display: flex;
  flex-direction: column;
  align-items: start;
  width: 100%;
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

.count {
  color: #EA0029;
  margin-bottom: 24px; 
  border: none;
  padding: 0;
  background: #fff;
}

.count:hover {
cursor:pointer;
}

.form_error input {
  border: 1px solid #ea0029;
}
</style>