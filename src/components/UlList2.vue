<script>
const LabeledEdit = {
  template: `
    <div class="labeled-edit">
      <label>
        <span class="label">{{ label }}</span>
        <input autofocus ref="input" v-model="value"
          @input="$emit('input', $event)" @keyup.enter="applyInput"
        />
      </label>
    </div>
  `,
  data: () => ({
    value: ''
  }),
  props: {
    data: Object,
    valueKey: String,
    label: String
  },
  methods: {
    applyInput(e) {
      const newData = { ...this.data, [this.valueKey]: this.value };
      this.$emit('save', newData);
    }
  },
  mounted() {
    this.value = this.data[this.valueKey];
    this.$refs.input.focus();
  },
  name: 'LabeledEdit'
};

const MyList = {
  template: `
    <div class="my-list">
      <div class="row" v-for="person in persons" :key="person.id">
        <span class="pid">{{ person.id + '.' }}</span>
        <labeled-edit v-if="person === personForEdit"
          :data="person" valueKey="name"
          @save="changePersonData(person, $event)"
        ></labeled-edit>
        <template v-else>
          <span class="name">{{ person.name }}</span>
          <button @click="personForEdit = person">Edit</button>
        </template>
      </div>
    </div>
  `,
  data: () => ({
    personForEdit: null
  }),
  props: {
    persons: { type: Array }
  },
  methods: {
    changePersonData(person, changedPerson) {
      const personIdx = this.persons.findIndex(p => p.id === person.id);
      if (personIdx < 0)
        throw new Error('No person found for changePersonName');
      this.persons.splice(personIdx, 1, { ...changedPerson });
      this.personForEdit = null;
    },
  },
  components: { LabeledEdit },
  name: 'MyList'
};

const 12321 = {
  template: `
    <div class="app">
      <div class="row create">
        <labeled-edit v-if="showEdit"
          :data="newPerson" valueKey="name" label="Создать:"
          @save="addPerson"
        ></labeled-edit>
        <button v-else class="btn-create" @click="createPerson">Create</button>
      </div>
      <hr>
      <my-list :persons="persons"></my-list>
    </div>
  `,
  data: () => ({
    showEdit: false,
    newPerson: null,
    persons: [
      { id: 0, name: 'Ivan' },
      { id: 1, name: 'Alex' },
      { id: 2, name: 'John' }
    ]
  }),
  methods: {
    createPerson() {
      const lastId = this.persons.reduce((r, p) => r = Math.max(r, p.id), -1);
      this.newPerson = { id: lastId + 1, name: '' };
      this.showEdit = true;
    },
    addPerson(person) {
      this.persons.push({ ...person });
      this.showEdit = false;
    }
  },
  components: { MyList, LabeledEdit },
  name: 'App'
};
</script>
