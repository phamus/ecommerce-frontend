<template>
  <div class="container-fluid">
    <div class="row">
      <div class="col-sm-3"></div>
      <div class="col-sm-6">
        <div class="a-section">
          <div class="a-spacing-top-medium"></div>
          <h2 style="text-align: center">Create Category</h2>

          <form>
            <div class="a-spacing-top-medium">
              <label>Type</label>
              <input
                v-model="type"
                type="text"
                name
                class="a-input-text"
                style="width:100%"
              />
            </div>
            <!-- button input  -->
            <div class="a-spacing-top-large">
              <span class="a-button-register">
                <span class="a-button-inner">
                  <span class="a-button-text" @click="onSubmit"
                    >Add Product</span
                  >
                </span>
              </span>
            </div>
          </form>
        </div>
      </div>
      <div class="col-sm-3"></div>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      type: ""
    };
  },
  methods: {
    async onSubmit() {
      let result = await this.$axios.$post(
        "http://localhost:5000/api/catgory",
        { type: this.type }
      );
      this.$router.push("/");
    }
  }
};
</script>
