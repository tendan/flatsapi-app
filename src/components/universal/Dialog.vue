<template>
  <v-dialog
    v-model="dialog"
    max-width="600px"
  >
    <template v-slot:activator="{ on, attrs }">
      <slot name="button" v-bind:btn="{ on, attrs }"></slot>
    </template>
    <v-card>
      <v-form
        ref="form"
        v-model="valid"
      >
        <v-card-title>
          <span class="text-h5">User Profile</span>
        </v-card-title>
        <v-card-text>
          <v-container>
            <v-row
              v-for="(parentField, i) in fields"
              :key="i"
            >
              <v-col
                v-for="(field, j) in parentField"
                :key="j"
                cols="12"
                sm="6"
                md="4"
              >
                <v-text-field
                  v-model="inputs[field.name]"
                  :label="field.name"
                  :rules="field.rules"
                ></v-text-field>
              </v-col>
            </v-row>
          </v-container>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn
            color="primary"
            text
            @click="dialog = false"
          >Anuluj</v-btn>
          <v-btn
            color="primary"
            @click="submit"
          >Dodaj</v-btn>
        </v-card-actions>
      </v-form>
    </v-card>
  </v-dialog>
</template>

<script>
export default {
  name: 'Dialog',
  data() {
    return {
      valid: null,
      dialog: false,
      rendered: false,
    };
  },
  props: {
    fields: {
      type: Array,
    },
    action: {
      type: Function,
    },
  },
  computed: {
    inputs() {
      return {};
    },
  },
  methods: {
    submit(e) {
      e.preventDefault();
      this.$refs.form.validate();
      if (this.valid) {
        this.$props.action(...Object.values(this.inputs));
        this.dialog = false;
        this.$refs.form.reset();
      }
    },
  },
};
</script>

<style scoped>

</style>
