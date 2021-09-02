<template>
  <form @submit.prevent="handleSubmit">
    <label>專案名稱:</label>
    <input type="text" v-model="title" required />
    <label>專案摘要:</label>
    <textarea v-model="details" required></textarea>
    <ul class="sub-btn">
      <button class="m-left">提交更新</button>
      <button class='m-right'>新增任務</button>
    </ul>
    
  </form>
</template>

<script>
export default {
  props: ["id"],
  data() {
    return {
      title: "",
      details: "",
      uri: "http://localhost:3000/projects/" + this.id,
    };
  },
  mounted() {
    fetch(this.uri)
      .then((res) => res.json())
      .then((data) => {
        this.title = data.title;
        this.details = data.details;
      });
  },
  methods: {
    handleSubmit() {
      fetch(this.uri, {
        method: "PATCH",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({ title: this.title, details: this.details }),
      })
        .then(() => {
          this.$router.push("/");
        })
        .catch((err) => console.log(err));
    },
  },
};
</script>

<style>
.sub-btn {
  display: flex;
  /* justify-content: space-between; */
  margin-left: 120px;
  margin-right: 170px;
}
</style>
