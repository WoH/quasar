<template>
  <div class="q-layout-padding">
    <div style="max-width: 600px" class="q-gutter-y-md">
      <h1>Input Validate</h1>

      <div class="q-gutter-sm">
        <q-radio v-model="type" val="filled" label="Filled" />
        <q-radio v-model="type" val="outlined" label="Outlined" />
        <q-radio v-model="type" val="standout" label="Standout" />
        <q-radio v-model="type" val="standard" label="Standard" />
        <q-radio v-model="type" val="borderless" label="Borderless" />
      </div>

      <div class="text-h6">
        Internal validation
        <q-btn label="Reset" @click="reset" color="primary" flat />
      </div>

      <q-input
        ref="input1"
        v-bind="{[type]: true}"
        v-model="model1"
        label="Label *"
        :rules="[
          val => !!val || '* Required'
        ]"
      />

      <q-input
        ref="input2"
        v-bind="{[type]: true}"
        v-model="model2"
        label="len <= 3"
        counter
        hint="Type something"
        :rules="[
          val => val.length <= 3 || 'Please use maximum 3 characters'
        ]"
      >
        <q-icon slot="append" name="close" @click="model2 = ''" class="cursor-pointer" />
      </q-input>

      <q-input
        ref="input3"
        v-bind="{[type]: true}"
        v-model="model3"
        label="Required, Lazy, Len < 2"
        counter
        hint="Validation starts after first blur"
        :rules="[
          val => !!val || '* Required',
          val => val.length < 2 || 'Please use maximum 1 character'
        ]"
        lazy-rules
      />

      <q-select
        v-bind="{[type]: true}"
        v-model="stringSingle"
        :options="stringOptions"
        label="Single - Required, Lazy"
        :rules="[
          val => !!val || '* Required',
        ]"
        lazy-rules
      />

      <q-select
        v-bind="{[type]: true}"
        v-model="stringSingle"
        :options="stringOptions"
        use-input
        label="Single - use-input - Required, Lazy"
        :rules="[
          val => !!val || '* Required',
        ]"
        lazy-rules
      />

      <q-field
        v-bind="{[type]: true}"
        label="Slider - >= 10, Lazy"
        stack-label
        :value="num"
        :rules="[
          val => val >= 10 || 'Select at least 10',
        ]"
        lazy-rules
      >
        <template v-slot:control>
          <q-slider
            class="q-mt-xl q-mx-md"
            v-model="num"
            :min="0"
            :max="50"
            label-always
          />
        </template>
      </q-field>

      <q-field
        v-bind="{[type]: true}"
        label="Date - required, Lazy"
        stack-label
        :value="date"
        :rules="[
          val => !!val || '* Required',
        ]"
        lazy-rules
      >
        <template v-slot:control>
          <q-date
            v-model="date"
          />
        </template>
      </q-field>

      <q-field
        v-bind="{[type]: true}"
        label="Time - required, Lazy"
        stack-label
        :value="time"
        :rules="[
          val => !!val || '* Required',
        ]"
        lazy-rules
      >
        <template v-slot:control>
          <q-time
            v-model="time"
          />
        </template>
      </q-field>

      <q-field
        v-bind="{[type]: true}"
        label="Knob - >= 10, Lazy"
        stack-label
        :value="num"
        :rules="[
          val => val >= 10 || 'Select at least 10',
        ]"
        lazy-rules
      >
        <template v-slot:control>
          <q-knob
            class="q-mt-md"
            v-model="num"
            color="black"
            center-color="grey-8"
            size="150px"
            show-value
            :min="0"
            :max="50"
          >
            {{ num }}
          </q-knob>
        </template>
      </q-field>

      <q-input
        ref="input3"
        v-bind="{[type]: true}"
        v-model="model4"
        label="Required, Len > 1, Len > 2"
        counter
        hint="Multiple"
        :rules="[
          val => !!val || '* Required',
          val => val.length > 1 || 'Please use min 1 characters',
          val => val.length > 2 || 'Please use min 2 characters'
        ]"
      />

      <q-input
        ref="input1"
        v-bind="{[type]: true}"
        v-model="model5"
        label="Multiple - call stack test *"
        :rules="[
          callRule1,
          void 0,
          callRule2
        ]"
      />

      <q-input
        ref="input1"
        v-bind="{[type]: true}"
        v-model="model6"
        label="Multiple - async call stack test *"
        :rules="[
          asyncCallRule1,
          asyncCallRule2
        ]"
      />

      <div class="text-h6 q-mt-xl">
        Async rules
      </div>
      <q-input
        ref="input1"
        v-bind="{[type]: true}"
        v-model="model7"
        label="Only async *"
        :rules="[
          asyncRule
        ]"
      />

      <q-input
        ref="input1"
        v-bind="{[type]: true}"
        v-model="model7"
        label="Multiple async *"
        :rules="[
          asyncRule,
          secondAsyncRule
        ]"
      />

      <q-input
        ref="input1"
        v-bind="{[type]: true}"
        v-model="model7"
        label="Loading slot *"
        :rules="[
          asyncRule
        ]"
      >
        <template v-slot:loading>
          <q-spinner-gears color="purple" />
        </template>
      </q-input>

      <q-input
        ref="input1"
        v-bind="{[type]: true}"
        v-model="model7"
        debounce="1000"
        label="X Mixed *"
        :rules="[
          asyncRule,
          val => val.length > 2 || 'Please use min 3 characters'
        ]"
      />

      <q-input
        ref="input1"
        v-bind="{[type]: true}"
        v-model="model7"
        debounce="1000"
        label="Debounced input *"
        :rules="[
          asyncRule
        ]"
      />

      <q-input
        ref="input1"
        v-bind="{[type]: true}"
        v-model="model7"
        label="Mixed, Lazy *"
        lazy-rules
        :rules="[
          asyncRule,
          val => val.length > 2 || 'Please use min 3 characters'
        ]"
      />

      <q-input
        ref="input1"
        v-bind="{[type]: true}"
        v-model="model7"
        label="Lazy async *"
        lazy-rules
        :rules="[
          asyncRule
        ]"
      />

      <div class="text-h6 q-mt-xl">
        External validation
      </div>
      <div class="q-gutter-sm">
        <q-toggle v-model="error" label="Error state" />
        <q-radio v-model="errorMessage" val="First error" label="First error" />
        <q-radio v-model="errorMessage" val="Second error" label="Second error" />
      </div>
      <q-input
        ref="inputExternal"
        v-bind="{[type]: true}"
        v-model="modelExternal"
        label="Label"
        hint="Hint"
        :error="error"
        :error-message="errorMessage"
      />

      <q-input
        ref="inputExternal"
        v-bind="{[type]: true}"
        v-model="modelExternal"
        label="Label"
        hint="Hint"
        :error="error"
        style="margin-bottom: 30px"
      >
        <div slot="error">
          Slotted error message
        </div>
        <div slot="error">
          Second slotted error message
        </div>
      </q-input>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    const n = 7

    const data = {
      n,
      type: 'filled',
      modelExternal: '',
      error: false,
      errorMessage: 'First error',
      stringSingle: null,
      stringOptions: [
        'Google', 'Facebook', 'Twitter', 'Apple', 'Oracle'
      ],
      num: 0,
      date: '',
      time: ''
    }

    for (let i = 1; i <= n; i++) {
      data['model' + i] = ''
    }

    return data
  },

  methods: {
    reset () {
      for (let i = 1; i <= this.n; i++) {
        this.$refs['input' + i].resetValidation()
      }
    },

    async asyncRule (val) {
      return new Promise((resolve, reject) => {
        setTimeout(() => {
          resolve(!!val || '* Required')
        }, 1000)
      })
    },

    async secondAsyncRule (val) {
      return new Promise((resolve, reject) => {
        setTimeout(() => {
          resolve((val && val.length > 2) || 'Min 3 characters')
        }, 1000)
      })
    },

    callRule1 (val) {
      console.log('call 1')
      return false
    },

    callRule2 (val) {
      console.log('call 2')
    },

    async asyncCallRule1 (val) {
      console.log('call async 1')
      return new Promise((resolve, reject) => {
        setTimeout(() => {
          reject(new Error('some err'))
          // resolve(!!val || '* Required 1')
        }, 1000)
      })
    },

    async asyncCallRule2 (val) {
      console.log('call async 2')
      return new Promise((resolve, reject) => {
        setTimeout(() => {
          resolve(!!val || '* Required 2')
        }, 1000)
      })
    }
  }
}
</script>
