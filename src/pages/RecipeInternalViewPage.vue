<template>
  <div  class="center" style="width: 90%;" v-if="recipes.length > 0">
    <br>
    <b-card
      no-body
      class="overflow-hidden"
      style="max-width: 90%;left:5%;right:8%"
    >
      <b-row no-gutters>
        <b-col md="6">
          <b-card-img
            :src="recipes[0].imageURL"
            alt="Image"
            class="rounded-0"
          ></b-card-img>
        </b-col>
        <b-col md="6">
          <b-card-body :title="recipes[0].name">
            <b-card-text>
              <b-list-group flush>
                <b-list-group-item>
                  <b-icon
                    icon="clock"
                    variant="dark"
                    style="margin-right:10px"
                  ></b-icon>
                  {{ recipes[0].cookingDuration }} Minutes
                </b-list-group-item>
                <b-list-group-item v-if="recipes[0].isVegan">
                  <img
                    src="https://img.icons8.com/color/25/000000/vegan-symbol.png"
                    style="margin-right:13px"
                  />Vegan</b-list-group-item
                >
                <b-list-group-item v-if="!recipes[0].isGluten">
                  <img
                    src="https://img.icons8.com/color/25/000000/no-gluten.png"
                    style="margin-right:13px"
                  />Gluten free</b-list-group-item
                >
                <b-list-group-item v-if="!recipes[0].isVegeterian">
                  <img
                    src="https://img.icons8.com/color/25/000000/vegetarian-mark.png"
                    style="margin-right:13px"
                  />Vegetarian</b-list-group-item
                >
                <b-list-group-item
                  >Number of dishes: {{ recipes[0].dishes }}</b-list-group-item
                >
                <b-list-group-item>
                  Ingredients:
                  <ul>
                    <li
                      v-for="ingred in recipes[0].ingredients"
                      :key="ingred.recipeID"
                    >
                      {{ ingred.name }} : {{ ingred.quantity }}
                      {{ ingred.unit }}
                    </li>
                  </ul>
                </b-list-group-item>
                <b-list-group-item>
                  <b>Instructions:</b>
                  {{ recipes[0].instructions }}
                </b-list-group-item>
              </b-list-group>
            </b-card-text>
          </b-card-body>
        </b-col>
      </b-row>
    </b-card>
  </div>
</template>

<script>
export default {
  data() {
    return {
      recipes: [],
    };
  },

  created() {
    if (this.$store.recipes) {
      this.recipes = this.$store.recipes.filter(
        (recipe) => recipe.recipeID == this.$route.params.recipeId
      );
    } else if (this.$root.store.username) {
      // getMyRecipes();
      // let link = 'https://assignment3-2-shiran-hen.herokuapp.com/user/myRecipes';
      let link = "http://localhost:3000/user/myRecipes";
      let response = this.axios
        .get(link)
        .then((res) => {
          this.myRecipes = res.data.message;
          if (this.myRecipes.length == 0) {
            this.myRecipes = false;
          } else {
            var dict = this.myRecipes;
            this.$store.recipes = dict;
          }
          this.recipes = this.$store.recipes.filter(
            (recipe) => recipe.recipeID == this.$route.params.recipeId
          );
        })
        .catch((err) => {
          this.$root.store.logout();
          this.$router.push("/login");
        });
    } else {
      this.$router.push("/login");
    }
  },
};
</script>

<style>
.center {
  display: block;
  margin-left: auto;
  margin-right: auto;
  width: 50%;
   /* background-color: rgba(60, 19, 224, 0.075); */
}</style>
