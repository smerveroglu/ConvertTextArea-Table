<template>
  <div id="input">
    <div id="editableTable">
      <b-button variant="danger" v-on:click="backEdit"
        ><strong>←Back</strong></b-button
      >
      <div id="buttons">
        <b-button variant="primary" v-on:click="download"
          ><strong>Download</strong></b-button
        >
        <b-button variant="primary" v-on:click="addRow" id="addRow"
          ><strong>Add Row</strong></b-button
        >
      </div>
      <br />
      <br />
      <b-table striped hover :items="items" :fields="fields" id="editTable">
        <template v-slot:cell()="{ item, field: { key } }">
          <b-form-input v-model="item[key]" />
        </template>
        <template v-slot:cell(action)="row">
          <b-button @click="removeItem(row.item)" variant="danger">
            Remove
          </b-button>
        </template>
      </b-table>
      <br />
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      value: this.$store.state.value,
      items: this.$store.state.items,
      fields: this.$store.state.columns,
    };
  },
  methods: {
    backEdit() {
      this.$store.state.isShow = !this.$store.state.isShow;
      this.$store.state.items = [];
    },
    removeItem(item) {
      const index = this.items.indexOf(item);
      confirm("Are you sure you want to delete this user?") &&
        this.items.splice(index, 1) &&
        this.value.splice(index, 1);
      console.log(this.value);
    },
    close() {
      this.modalShow = false;
      setTimeout(() => {
        this.editedItem = Object.assign({}, this.formFields);
        this.editedIndex = -1;
      }, 200);
    },
    addRow() {
      let s = new Object();
      this.$store.state.columns.forEach((e) => {
        s[e] = "";
      });
      this.items.push(s);
    },
    download() {
      const data = this.items;
      console.log(data);
      let csv = "";
      let c = 0;
      //Add Key with ',' in csv
      Object.keys(data[0]).forEach((key) => {
        if (key == "Action") {
          c++;
        } else {
          csv += key + ",";
        }
      });
      csv = csv.substring(0, csv.length - 1);
      csv += "\n";

      //Add Value with ',' in csv
      data.forEach(function (d) {
        var row = [];

        Object.keys(d).forEach((key) => {
          if (key == "Action") {
            c++;
          } else {
            row.push(d[key]);
          }
        });
        csv += row.join(",");
        csv += "\n";
      });

      //Create and Download CSV file
      let element = document.createElement("a");
      element.href = "data:text/csv;charset=utf-8," + encodeURI(csv);
      element.target = "_blank";
      element.download = "Student.csv";
      element.click();
    },
  },
};
</script>

<style>
#editableTable {
  width: 700px;
  height: 100px;
}
#table {
  width: 700px;
  height: 100px;
}
#buttons {
  margin-left: 460px;
  margin-top: -40px;
}
</style>