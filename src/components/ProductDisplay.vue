<template>
  <div class="container">
    <div v-if="isLoading" class="card">
      <div class="product-container">
        <div class="err-img"></div>
        <div class="err-body">
          <div class="err-head"></div>
          <div class="err-footer"></div>
        </div>
      </div>
    </div>
    <div 
    v-else
    class="container"
    :class="{
      'bg-cyan': menCategory,
      'bg-graylight': unlabelCategory,
      'bg-pink': womenCategory,
    }" >
    <div v-if="!unlabelCategory" class="overlayer">
    <img src="../assets/bg-pattern.svg" alt="thisbgpatern">
    </div>
    <div class="card">
      <div v-if="unlabelCategory" class="unlabel-contain" >
      <div class="overlayer">
        <img src="../assets/sad-face.svg" alt="unlabelproduk"/>
      </div>
      <div class="card-body">
        <p>This product is unavailable to show</p>
        <div class="button-click">
          <button @click="nextProduct" class="btn-nxt" >
          Next Product
        </button>
        </div>
      </div>
      </div>
      <div v-else class="product-container">
        <div class="card-imgs">
          <img :src="product?.image" alt="product-img"/>
        </div>
        <div class="card-bdy">
          <div class="card-body-head">
            <h2 
            class="title"
            :class="menCategory ? 'text-navy' : 'text-purple'"
            >
          {{ product?.title }}
          </h2>
          <div class="card-body-head-subs">
            <p> {{ product?.category }} </p>
            <div class="rating">
              <span>{{ product?.rating.rate }}</span>
              <div class="circle-rate">
                <span
                class="circle"
                :class="
                  product?.rating.rate > 0.5 || product?.rating.rate < 1.5
                  ? menCategory
                  ? 'bg-navy'
                  : 'bg-purple'
                  :menCategory
                  ?'bg-navy-white'
                  : 'bg-purple-white'
                "
                >
                </span>
                <span 
                class="circle"
                :class="
                  product?.rating.rate >= 1.5
                  ? menCategory
                  ? 'bg-navy'
                  : 'bg-purple'
                  :menCategory
                  ?'bg-navy-white'
                  :'bg-purple-white'
                "
                >
                </span>
                <span 
                class="circle"
                :class="
                  product?.rating.rate >= 2.5
                  ? menCategory
                  ? 'bg-navy'
                  : 'bg-purple'
                  : menCategory
                  ?'bg-navy-white'
                  :'bg-purple-white'
                "
                >
                </span>
                <span 
                class="circle"
                :class="
                  product?.rating.rate >= 3.5
                  ? menCategory
                  ? 'bg-navy'
                  : 'bg-purple'
                  :menCategory
                  ?'bg-navy-white'
                  :'bg-purple-white'
                "
                >
                </span>
                <span 
                class="circle"
                :class="
                  product?.rating.rate >= 4.5
                  ? menCategory
                  ? 'bg-navy'
                  : 'bg-purple'
                  :menCategory
                  ?'bg-navy-white'
                  :'bg-purple-white'
                "
                >
                </span>
              </div>
            </div>
          </div>
          </div>
          <div class="des">
            <p>{{ product?.description }}</p>
          </div>
          <div class="card-footer">
            <span
            class="price"
            :class="menCategory ? 'text-navy' : 'text-purple'"
            >$ {{ product?.price }}</span>
            <div class="button-click">
              <button class="btn-buying" 
              :class="menCategory ? 'bg-navy' : 'bg-purple'"
              >
              Buy Now
            </button>
            <button
              class="btn-nxt"
              :class="
                menCategory
                ? 'border-navy text-navy'
                : 'border-purple text-purple'
              "
              @click="nextProduct"
            >
            Next Product
          </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</div>
</div>


 </template>

 <script lang="ts">
    import { defineComponent } from 'vue';
    import axios from 'axios';

    export default defineComponent({
      data(){
        return {
          productIndex: 16,
          product: null as any,
          isLoading: true,
          menCategory: false,
          womenCategory: false,
          unlabelCategory: false,
          maxProductIndex: 20,
        };
      },
      async mounted(){
        await this.fetchProduct();
      },
      methods: {
        async fetchProduct(){
          try {
            this.isLoading = true;
            let res = await axios.get(`https://fakestoreapi.com/products/${this.productIndex}`);
            if (res.data.category === "men's clothing"){
              this.menCategory = true;
              this.womenCategory = false;
              this.unlabelCategory = false;
              this.product = res.data;
            } else if ( res.data.category === "women's clothing"){
              this.menCategory = false;
              this.womenCategory = true;
              this.unlabelCategory = false;
              this.product = res.data;
            } else {
              this.menCategory = false;
              this.womenCategory = false;
              this.unlabelCategory = true;
              this.product = null;
            }
          }
            catch (err) {
              console.error(err);
            } finally {
              this.isLoading = false;
            }
          },
          nextProduct(){
            this.productIndex++;
            if (this.productIndex > this.maxProductIndex){
              this.productIndex = 1;
            }
            this.fetchProduct();
          },
        },
    });
 </script>

