<template>
<div class="wrapper-popup" :class="getClassPopup">
  <div class="rule">
    <h3>Luật chơi</h3>
    <p>
      Lorem ABC
    </p>
    <button v-on:click="confirmPopup" class="confirm" >Đã hiểu</button>
  </div>
</div>
</template>

<script>
export default {
  name: "PopupRule",
  props: {
    isOpenPopup: {type: Boolean, default: false}
  },
  data() {
    return {
    }
  },
  computed: {
    getClassPopup() {
      return {
        "popup-open": this.isOpenPopup
      }
    }
  },
  methods: {
    confirmPopup() {
      // truyen event ra App.vue
      console.log("HandleConfirm PopupRule.vue");
      this.$emit("handleConfirm")
    }
  }
}
</script>

<style scoped>
  .wrapper-popup {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background-color: rgba(0,0,0,0.4);
    opacity: 0;
    visibility: hidden;
    transition: all .3s ease;
  }
  .wrapper-popup.popup-open {
    opacity: 1;
    visibility: visible;
  }
  .rule {
    width: 350px;
    padding: 20px;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%) scale(1.2);
    background-color: #fff;
    position: absolute;
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    transition: all .3s ease;
  }
  .popup-open .rule{
    transform: translate(-50%, -50%) scale(1);
  }
  .rule h3 {
    margin-bottom: 10px;
  }
  .rule .confirm {
    cursor: pointer;
    margin-top: 20px;
    padding: 8px 15px;
    border: 2px solid #333;
    background-color: #fff;
    transition: all .3s ease;
  }
  .rule .confirm:hover {
    color: #fff;
    background-color: #333;
  }
</style>
