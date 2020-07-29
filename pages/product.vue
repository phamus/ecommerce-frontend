<template>
  <div class="container-fluid">
    <div class="row">
      <div class="col-sm-3"></div>
      <div class="col-sm-6">
        <div class="a-section">
          <div class="a-spacing-top-medium"></div>
          <h2 style="text-align:center">Add a new Product</h2>
          <form>
            <!-- category drop down  -->
            <div class="a-spacing-top-medium">
              <label>Category</label>
              <select v-model="categoryID" name id class="a-select-option">
                <option
                  v-for="category in categories"
                  :key="category._id"
                  :value="category._id"
                  >{{ category.type }}</option
                >
              </select>
            </div>

            <!-- owner drop down  -->
            <div class="a-spacing-top-medium">
              <label>Owner</label>
              <select v-model="ownerID" name id class="a-select-option">
                <option
                  v-for="owner in owners"
                  :key="owner._id"
                  :value="owner._id"
                  >{{ owner.name }}</option
                >
              </select>
            </div>

            <!-- title input  -->
            <div class="a-spacing-top-medium">
              <label>Title</label>
              <input
                v-model="title"
                type="text"
                name
                class="a-input-text"
                style="width:100%"
              />
            </div>

            <!-- price input  -->
            <div class="a-spacing-top-medium">
              <label>Price</label>
              <input
                v-model="price"
                type="number"
                name
                class="a-input-text"
                style="width:100%"
              />
            </div>

            <!-- stockQuantity input  -->
            <div class="a-spacing-top-medium">
              <label>StockQuantity</label>
              <input
                v-model="StockQuantity"
                type="number"
                name
                class="a-input-text"
                style="width:100%"
              />
            </div>

            <!-- description input  -->
            <div class="a-spacing-top-medium">
              <label>Description</label>
              <textarea
                placeholder="Provide details such as a product description"
                style="width:100%"
                v-model="description"
              ></textarea>
            </div>

            <!-- photo input  -->
            <div class="a-spacing-top-medium">
              <label>Photo</label>
              <div class="a-row a-spacing-top-medium">
                <label for class="choosefile-button">
                  <i class="fal fa-plus"></i>
                </label>
                <input type="file" @change="onFileSelected" />
                <p style="margin-top:-70px">{{ fileName }}</p>
              </div>
            </div>

            <hr />
            <!-- button input  -->
            <div class="a-spacing-top-large">
              <span class="a-button-register">
                <span class="a-button-inner">
                  <span class="a-button-text" @click="onAddProduct"
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
  async asyncData({ $axios }) {
    let categories = $axios.$get("http://localhost:5000/api/category");
    let owners = $axios.$get("http://localhost:5000/api/owner");

    const [catResponse, ownerResponse] = await Promise.all([
      categories,
      owners
    ]);
    return {
      categories: catResponse.categories,
      owners: ownerResponse.owners
    };
  },

  data() {
    return {
      categoryID: "",
      ownerID: "",
      title: "",
      price: 0,
      description: "",
      selectedField: null,
      StockQuantity: 1,
      fileName: ""
    };
  },
  methods: {
    onFileSelected(event) {
      this.selectedField = event.target.files[0];
      console.log(this.selectedField);
      this.fileName = event.target.files[0].name;
    },
    async onAddProduct() {
      const file = new FormData();
      file.append("title", this.title);
      file.append("price", this.price);
      file.append("description", this.description);
      file.append("ownerId", this.ownerID);
      file.append("categoryId", this.categoryID);
      file.append("StockQuantity", this.StockQuantity);
      file.append("photo", this.selectedField, this.selectedField.name);

      let result = await this.$axios.$post(
        "http://localhost:5000/api/product/product",
        file
      );
      this.$router.push("/");
    }
  }
};
</script>
