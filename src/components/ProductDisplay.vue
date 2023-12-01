<template>
  <div class="productContent" id="productContent">
    <div class="bg" id="bg">
      <div class="color-bg1" id="bg1" :style="{ backgroundColor: getCategoryColorBg(category) }"></div>
      <div class="color-bg2" id="bg2"></div>
    </div>
    <!-- looping data dalam array menWomenProdcut untuk diatmpilkan pada component -->
    <div v-if="menWomenProducts.length > 0">
      <div
        class="content-box"
        id="content-box"
        v-for="product in menWomenProducts"
        :key="product.id"
      >
        <div class="gambar ">
          <img :src="getProductImage(product)" alt="productPicture" />
        </div>
        <div class="information">
          <h2 :style="{ color: getCategoryColorButton(category) }">
            {{ product.title }}
          </h2>
          <div class="rating">
            <span class="category">{{ product.category }}</span>
            <span class="itemRating">2.9/5</span>
          </div>
          <hr />
          <p>
            {{ product.description }}
          </p>
          <hr class="" style="margin-top: 50px" />
          <h2 :style="{ color: getCategoryColorButton(category) }">${{ product.price }}</h2>
          <div class="buttonProduct ">
            <button class="Buy" @click="buyProduct(product)" :style="{ backgroundColor: getCategoryColorButton(category) }">Buy Now</button>
            <button class="Next" @click="getNextProducts" :style="{ borderColor: getCategoryColorButton(category) }">Next Product</button>
          </div>
        </div>
      </div>
    </div>
    <!-- jika bukan category men dan women, munculkan unavailable product -->
    <div v-else-if="category !== 'men\'s clothing' && category !== 'women\'s clothing'">
      <div class="content-box-unavailable" id="content-box-unavailable">
        <div class="unavailable-product ">
          <h2>This product is unavailable to show</h2>
          <button class="Next" @click="getNextProducts">Next Product</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      menWomenProducts: [],
      currentIndex: 1,
    };
  },
  mounted() {
    this.fetchProducts();
  },
  methods: {
  //  Fungsi dalam membedakan warna untuk background dan Judul berdasarkan categorynya
    getCategoryColorBg(category) {
      return category === "men's clothing" ? '#d6e6ff' :
             category === "women's clothing" ? '#fde2ff' :
             '#dcdcdc'; // Gaya default jika kategori tidak sesuai
    },
    getCategoryColorButton(category) {
      return category === "men's clothing" ? '#720060' :
             category === "women's clothing" ? '#002772' :
             '#1e1e1e'; // Gaya default jika kategori tidak sesuai
    },
    // fetching API dari fakestore
    fetchProducts() {
      const apiUrl = `https://fakestoreapi.com/products/${this.currentIndex}`;
      axios
        .get(apiUrl)
        .then((response) => {
          const products = Array.isArray(response.data)
            ? response.data
            : [response.data];

          // Set variabel category
          this.category = products[0]?.category.toLowerCase() || "";

          // Filter produk sesuai dengan kategori yang diinginkan
          const menWomenProducts = products.filter(
            (product) =>
              product.category === "men's clothing" ||
              product.category === "women's clothing"
          );

          // Simpan produk hanya jika kategori sesuai
          if (menWomenProducts.length > 0) {
            this.menWomenProducts = menWomenProducts;
          } else {
            // Jika tidak ada produk yang sesuai, tampilkan tulisan "Data Kosong"
            this.menWomenProducts = [
            ];
          }
        })
        .catch((error) => {
          console.error("Error fetching data:", error);
        });
    },
    // fungsi dalam melakukan Next prodcut dan kembali ke produk awal jika index telah mencapai 20
    getNextProducts() {
      this.currentIndex++;
      if (this.currentIndex > 20) {
        this.currentIndex = 1;
      }
      this.fetchProducts();
    },
    buyProduct(product) {
      console.log(`Product ${product.title} has been bought!`);
      window.alert("Success to Buy !");
    },
    getProductImage(product) {
      return product && product.image ? product.image : "";
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
@import "@/assets/style/page.css";

#productContent {
  position: relative;
}

#bg1 {
  background-color: var(--bg-color-women);
  height: 70vh;
  background-image: url("../assets/bg-pattern.png");
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
}
#bg2 {
  background-color: whitesmoke;
  height: 30vh;
} 

h1 {
  margin: 0;
  padding: 0;
}

#content-box {
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  margin: auto;
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: #ffffff;
  text-align: center;
  width: 70%;
  height: fit-content;
  border-radius: 15px;
  box-shadow: rgba(0, 0, 0, 0.24) 0px 5px 8px;
  padding: 10px;
}

#content-box-unavailable {
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  margin: auto;
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: #ffffff;
  text-align: center;
  width: 70%;
  height: fit-content;
  border-radius: 15px;
  box-shadow: rgba(0, 0, 0, 0.24) 0px 5px 8px;
  padding: 30px;
  background-image: url("../assets/sad-face.png");
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
  
}

.gambar {
  width: 30%;
  align-items: center;
  justify-content: center;
  text-align: left;
}
.gambar img {
  width: 250px;
}

.unavailable-product {
  align-items: center;
  justify-content: center;
  text-align: center;
  margin: 150px;
}

.unavailable-product .Next {
  border-radius: 4px;
  border: 3px solid #720060;
  background: #fff;
  color: #720060;
  font-family: "Inter", "Helvetica";
  font-size: 20px;
  font-style: normal;
  font-weight: 600;
  line-height: normal;
  width: 45%;
  padding: 10px;
  margin: 10px;
}
.information {
  width: 50%;
  align-items: center;
  justify-content: center;
  text-align: left;
  padding: 20px;
}
.information h2 {
  color: #720060;
  font-family: "Inter", "Helvetica";
  font-size: 28px;
  font-style: normal;
  font-weight: 600;
  line-height: normal;
}

.rating {
  display: flex;
  justify-content: space-between;
  font-family: "Inter", "Helvetica";
}

.information p {
  color: #1e1e1e;
  font-family: Inter;
  font-size: 20px;
  font-style: normal;
  font-weight: 400;
  line-height: normal;
  font-family: "Inter", "Helvetica";
}

.buttonProduct {
  display: flex;
  justify-content: center;
  text-align: center;
}

.buttonProduct .Buy {
  border-radius: 4px;
  background: #720060;
  color: #fff;
  font-family: "Inter", "Helvetica";
  font-size: 20px;
  font-style: normal;
  font-weight: 600;
  line-height: normal;
  width: 45%;
  padding: 10px;
  margin: 10px;
}

.buttonProduct .Next {
  border-radius: 4px;
  border: 3px solid #720060;
  background: #fff;
  color: #720060;
  font-family: "Inter", "Helvetica";
  font-size: 20px;
  font-style: normal;
  font-weight: 600;
  line-height: normal;
  width: 45%;
  padding: 10px;
  margin: 10px;
}
</style>
