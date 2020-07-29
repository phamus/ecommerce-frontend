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
                    >Add Category</span
                  >
                </span>
              </span>
            </div>
          </form>

          <hr />
          <ul class="list-group-item">
            <li
              v-for="category in categories"
              :key="category._id"
              style="border-bottom:1px solid #0000001a; padding-bottom:20px;padding-top:8px"
            >
              {{ category.type }}
            </li>
          </ul>
        </div>
      </div>
      <div class="col-sm-3"></div>
    </div>
  </div>
</template>
<script>
export default {
  async asyncData({ $axios }) {
    let categories = await $axios.$get("http://localhost:5000/api/category");

    return categories;
  },
  data() {
    return {
      type: ""
    };
  },
  methods: {
    async onSubmit() {
      try {
        const data = { type: this.type };
        let result = await this.$axios.$post(
          "http://localhost:5000/api/category",
          data
        );

        this.categories.push(data);
      } catch (error) {
        console.log(error);
      }
    }
  }
};
</script>
