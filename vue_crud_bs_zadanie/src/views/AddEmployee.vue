<template>
  <div>
    <b-modal id="addNewEmployee" title="Dodaj pracownika">
      <b-form>
        <b-form-group
          id="input-group-1"
          label="ID pracownika:"
          label-for="input-1">
          <b-form-input
            id="input-1"
            v-model="user.id"
            type="text"
            required
            :disabled="user.isEdit"
            placeholder="Dodaj unikalny identyfikator pracownika">
          </b-form-input>
          <span v-if="submitted && $v.user.id.$error" class="errorMsg">Wprowadz identyfikator pracownika</span>
        </b-form-group>
        <b-form-group
          id="input-group-2"
          label="`Imię i nazwisko"
          label-for="input-2">
          <b-form-input
            id="input-2"
            v-model="user.name"
            type="text"
            required
            placeholder="Wpisz imię i nazwisko nowego pracownika">
          </b-form-input>
          <span v-if="submitted && $v.user.name.$error" class="errorMsg">Wpisz nazwisko nowego pracownika</span>
        </b-form-group>
        <b-form-group
          id="input-group-3"
          label="Email:"
          label-for="input-3">
          <b-form-input
            id="input-3"
            v-model="user.email"
            type="email"
            required
            placeholder="Wpisz adres Email">
          </b-form-input>
          <div v-if="submitted && $v.user.email.$error" class="errorMsg">
            <span v-if="!$v.user.email.required">Uzupełnij adres email</span>
            <span v-if="$v.user.email && !$v.user.email.email">Wpisz poprawny adres email.</span>
          </div>
        </b-form-group>
        <b-form-group
          id="input-group-4"
          label="Numer telefonu:"
          label-for="input-4">
          <b-form-input
            id="input-4"
            v-model="user.contact"
            type="text"
            required
            placeholder="Uzupełnij dane kontaktowe">
          </b-form-input>
          <span v-if="submitted && $v.user.contact.$error" class="errorMsg">
            <span v-if="!$v.user.contact.required">Uzupełnij numer telefonu</span>
            <span v-if="!$v.user.contact.minLength">Numer powinien zawierać minimum 9 znaków</span>
            <span v-if="!$v.user.contact.maxLength">Numer powinien zawierać maksymalnie 14 znaków</span>
          </span>
        </b-form-group>
      </b-form>
      <template v-slot:modal-footer>
        <div class="w-100">
          <b-button
            variant="danger"
            size="sm"
            class="float-right m-l-10"
            @click="submitForm">
            Zapisz
          </b-button>
          <b-button
            variant="primary"
            size="sm"
            class="float-right"
            @click="resetForm">
            Nie zapisuj
          </b-button>
        </div>
      </template>
    </b-modal>
  </div>
</template>

<script>
import { BModal, BForm } from 'bootstrap-vue'
import { required, minLength, maxLength, email } from 'vuelidate/lib/validators'

export default {
  name: 'add_new_employee',
  data () {
    return {
      user: {
        id: '',
        name: '',
        email: '',
        contact: ''
      },
      submitted: false
    }
  },
  validations: {
    user: {
      id: { required },
      name: { required },
      email: { required, email },
      contact: { required, minLength: minLength(10), maxLength: maxLength(12) }
    }
  },
  mounted () {
    this.$root.$on('edit-employee', (data) => {
      this.user = data
      this.user.isEdit = true
      this.submitted = false
    })
    this.$root.$on('add-employee', (data) => {
      this.user = {}
      this.user.isEdit = false
      this.submitted = false
    })
  },
  components: {
    'b-modal': BModal,
    'b-form': BForm
  },
  methods: {
    submitForm () {
      this.submitted = true
      this.$v.$touch()
      if (this.$v.$invalid) {
        return
      }
      this.$bvModal.hide('addNewEmployee')
      this.$store.dispatch('updatelistEmployee', { employee: this.user }) // dispatch store action
    },
    resetForm () {
      this.user = {}
      this.user.isEdit = false
      this.submitted = false
    }
  }
}
</script>

<style>

</style>
