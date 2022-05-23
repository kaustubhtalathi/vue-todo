<template>
  <div class="container">
    <form @submit.prevent="onSubmit">
      <div class="row g-3">
          <div class="col-sm-4">
            <select class="form-select" v-model.number="priority">
              <option value="LIFE_CHANGING">Life changing</option>
              <option value="IMPORTANT">Important</option>
              <option value="MEH">Meh</option>
            </select>
          </div>
          <div class="col-sm-6">
            <input type="text" class="form-control" placeholder="Enter task" maxlength="140" v-model="inputTask" />
          </div>
          <div class="col-sm-2">
            <button type="submit" class="btn btn-success" :disabled="isSubmitDisabled">Submit</button>
          </div>
      </div>
    </form>
  </div>
</template>

<script>
export default {
  name: 'TaskForm',
  props: {
    title: String
  },
  data() {
    return {
      inputTask: "",
      priority: "LIFE_CHANGING"
    }
  },
  methods: {
    onSubmit() {
      const task = {
        text: this.inputTask,
        priority: this.priority
      }
      this.$emit("submit-task", task);

      this.inputTask = "";
      this.priority = "LIFE_CHANGING";
    }
  },
  computed: {
    isSubmitDisabled() {
      console.log(this);
      return this.inputTask === ""; 
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.container {
  max-width: 760px;
}

.row {
  text-align: center;
}
</style>
