<template>
  <q-input ref="field" :model-value="displayValue" :dense="dense" :outlined="outlined" :label="label" reactive-rules
           lazy-rules :rules="rules">
    <template #append>
      <q-btn dense unelevated outline color="secondary" icon="access_time">
        <q-popup-proxy ref="qTimeProxy">
          <q-time minimal :model-value="value" @update:model-value="onUpdateValue" :mask="valueFormat" emit-immediately
                  :minute-options="[0, 5, 10, 15, 20, 25, 30, 35, 40, 45, 50, 55]"
                  format24h>
            <div class="row items-center justify-end">
              <q-btn v-close-popup label="Закрыть" color="primary" flat/>
            </div>
          </q-time>
        </q-popup-proxy>
      </q-btn>
    </template>
  </q-input>
</template>

<script>
import {useFormChild, date} from 'quasar'
import {i18n} from '../../boot/i18n'
import {defineComponent} from 'vue'

export default defineComponent({
  name: "CrudTime",
  inheritAttrs: false,
  props: {
    rules: {
      type: Array,
      default: () => []
    },
    modelValue: String,
    label: String,
    outlined: {
      type: Boolean,
      default: true
    },
    dense: {
      type: Boolean,
      default: true
    },
    customDisplayFormat: {
      type: String
    },
    customValueFormat: {
      type: String
    }

  },
  data: () => ({
    value: null
  }),
  computed: {
    valueFormat() {
      //Если в базе dataTime поле хранится экзотически, то берем эту маску:
      if (this.customValueFormat) {
        return this.customValueFormat
      }
      return 'HH:mm:ss'
    },
    displayValue() {
      let mask = i18n.global.t('maskLocaleTime')
      if (this.customDisplayFormat) {
        mask = this.customDisplayFormat
      }
      if (this.modelValue){
        const dateObj = date.extractDate(this.modelValue, 'HH:mm:ss')
        return date.formatDate(dateObj, 'HH:mm')
      }
      return undefined

    }
  },
  mounted() {
    useFormChild({validate: this.validate, resetValidation: this.resetValidation, requiresQForm: true})
    this.value = this.modelValue
  },
  methods: {
    onUpdateValue(value, reason, details) {
      this.value = value
      this.$emit('update:model-value', value);
    },
    validate(...args) {
      return this.$refs.field.validate(this.modelValue)
    },
    resetValidation() {
      // console.warn('resetValidation')
    }
  }
})
</script>
