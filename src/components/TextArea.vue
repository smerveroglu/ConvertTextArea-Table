<template>
  <div id="textarea">
    <b-form-textarea
      id="textarea-formatter"
      v-model="value"
      placeholder="Example: johndoe,johndoe@codementum.com,John,Doe,123"
    >
    </b-form-textarea>
    <br />
    <b-button block variant="primary" v-on:click="addValue" id="continue"
      >Continue</b-button
    >
  </div>
</template>

<script>
export default {
  data() {
    return {
      value: this.$store.state.value,
      items: this.$store.state.items,
      student: [],
    };
  },
  computed: {
    list() {
      return this.value.split("\n");
    },
  },
  methods: {
    addValue() {
      this.$store.state.isShow = !this.$store.state.isShow;
      this.createColumns();
      this.list.forEach((element) => {
        let splitter = this.chooseSplitter(element);
        let student = element.split(splitter);
        let s = new Object();
        let i = 0;
        this.$store.state.columns.forEach((e) => {
          s[e] = student[i];
          i++;
        });
        this.items.push(s);
      });
    },
    createColumns() {
      let max = 0;
      this.list.forEach((element) => {
        let splitter = this.chooseSplitter(element);
        let student = element.split(splitter);
        if (max < student.length) {
          max = student.length;
        }
      });
      this.$store.state.columns = Array.from(
        { length: max },
        (_, i) => "Field-" + ++i
      );
      this.$store.state.columns[max] = "Action";
      return this.$store.state.columns;
    },
    chooseSplitter(student) {
      let splitter = "?";
      if (student.includes("\t")) {
        splitter = "\t";
      } else if (student.includes(";")) {
        splitter = ";";
      } else if (student.includes(",")) {
        splitter = ",";
      }
      return splitter;
    },
  },
};
</script>
<style >
#textarea-formatter {
  width: 700px;
  height: 100px;
}
#continue {
  width: 700px;
}
</style>