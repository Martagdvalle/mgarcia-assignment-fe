<template>
   
  <div class="jumbotron vertical-center">
      <div class = "container">
        <nav>
    <ul class="nav">
      <li>
        <a href="/">Home </a>
      </li>
      <li>
        <a href="/recipe">Recipe</a>
      </li>
    </ul>
  </nav>
          <div class = "row">
              <div class = "col-sm-12">
                  <h1>Marta's Recipes Book</h1>
                  <hr />
                  <!-- Allert Message -->
        <b-alert v-if="showMessage" variant="success" show>{{
          message
        }}</b-alert>
        <!-- b-alert v-if="error" variant="danger" show>{{ error }}</b-alert-->

        <button
          type="button"
          class="btn btn-dark btn-sm"
          v-b-modal.recipe-modal
        >
          Create 
        </button>
                  <br /> <br />
                  <table class = "table table-hover">
                      <thead>
                          <tr>
                              <th scope = "col"> Recipe Name </th>
                              <th scope = "col"> Recipe Ingredients </th>
                              <th scope = "col"> Recipe Instructions </th>
                              <th scope = "col"> Recipe Favorite </th>
                              <th scope = "col"> Recipe Rating </th>
                          </tr>
                      </thead>
                      <tbody>
                          <tr v-for= "recipe in recipes" :key = "recipe.id">
                              <td> {{ recipe.name }} </td>
                              <td> {{ recipe.ingredients }} </td>
                              <td> {{ recipe.instructions }} </td>
                              <td>
                                  <b-icon 
                                  v-if="recipe.favorite == true" icon="heart-fill">
                                  </b-icon>
                                  <b-icon v-if="(recipe.favorite == false)" icon="heart">
                                  </b-icon>
                              </td>
                                <td> 
                                    <div>
                                        <b-form-rating v-model= "recipe.rating" readonly variant="warning" class="mb-2"></b-form-rating>
                                        <p class="mt-2"></p>
                                    </div> 
                                 </td>
                              
                               <td> 
                                  <div class="btn-group" role="group">
                                      <button
                                      type="button"
                                      class="btn btn-info btn-sm"
                                      v-b-modal.edit-recipe-modal
                                      @click="editRecipe(recipe)"
                                      >
                                      Edit
                                      </button>
                                      <button
                                      type="button"
                                      class="btn btn-danger btn-sm"
                                      @click="deleteRecipe(recipe)"
                                      >
                                      Delete
                                      </button>
                                  </div>
                              </td>
                          </tr>
                      </tbody>
                  </table>
                  <footer class = "text-center">
                  </footer>
              
              </div>
          </div>
          <!-- Beginning of Modal for Create -->
              <div>   <b-modal 
              ref = "addRecipeModal"
              id="recipe-modal"
              title="Add Recipe"
              hide-backdrop
              hide-footer
              >
              <b-form @submit="onSubmit" class="w-100">
              <b-form-group
                  id="name"
              label="Recipe Name:"
              label-for="name"
              description="Name of the recipe."
              >
              <b-form-input
              id="name"
              type="text"
              v-model="createRecipeForm.name"
              placeholder="name"
              required
              >
              </b-form-input>
              </b-form-group>
              <b-form-group
              id="ingredients"
              label="Recipe Ingredients:"
              label-for="ingredients"
              description="Ingredients of the recipe."
              >
              <b-form-input
              id="ingredients"
              type="text"
              v-model="createRecipeForm.ingredients"
              placeholder="ingredients"
              required
              ></b-form-input>
              </b-form-group>
              <b-form-group
              id="instructions"
              label="Recipe Instructions:"
              label-for="instructions"
              description="Instructions of the recipe."
              >
              <b-form-input
              id="instructions"
              type="text"
              v-model="createRecipeForm.instructions"
              placeholder="Recipe Instructions"
              required
              ></b-form-input>
              </b-form-group>
              <b-form-group
              id="form-favorite-group"    
              label="Recipe Favorite:"
              label-for="form-edit-favorite-input"
              description="favorite this recipe"
              >
              <b-form-checkbox
              id="form-favorite-input"
              v-model="createRecipeForm.favorite"
              switch
              ></b-form-checkbox>
              </b-form-group>
              <b-form-group
              id="rating"
              label="Recipe Rating:"
              label-for="form-rating-input"
              description="Rate this recipe from 1-5"
              >
              <b-form-rating
              id="rating"
              type="integer"
              v-model="createRecipeForm.rating"
              ></b-form-rating>
              </b-form-group>
              <b-button type="submit" variant="primary">Submit</b-button>
              </b-form>
          </b-modal>

          <!-- End of Modal for Create Recipe-->

          <!-- Beginning of Modal for Edit Recipe-->
          <b-modal
          ref = "editRecipeModal"
          id="edit-recipe-modal"
          title="Edit Recipe"
          hide-backdrop
          hide-footer
          >
          <b-form @submit="onSubmitUpdate" class="w-100">
          <b-form-group
          id="form-name-group"
          label="name:"
          label-for="form-name-input"
          description="Enter the name of the recipe."
          >
          <b-form-input
          id="form-name-input"
          type="text"
          v-model="updateRecipeForm.name"
          placeholder="name"
          
          >
          </b-form-input>
          </b-form-group>
          <b-form-group
          id="form-ingredients-group"
          label="ingredients:"
          label-for="form-ingredients-input"
          description="Enter the ingredients of the recipe."
          >
          <b-form-input
          id="form-ingredients-input"
          type="text"
          v-model="updateRecipeForm.ingredients"
          placeholder="ingredients"
          >
          </b-form-input>
          </b-form-group>
          <b-form-group
          id="form-instructions-group"
          label="Recipe Instructions:"
          label-for="form-instructions-input"
          description="Enter the instructions of the recipe."
          >
          <b-form-input

          id="form-instructions-input"
          type="text"
          v-model="updateRecipeForm.instructions"
          placeholder="Recipe Instructions"
          >
          </b-form-input>
          </b-form-group>

          <b-form-group

          id="form-favorite-group"
          label="Recipe Favorite:"
          label-for="form-favorite-input"
          description="Let us know if you want to favorite this recipe"
          >
          <b-form-checkbox
          id="form-favorite-input"
          v-model="updateRecipeForm.favorite"
          switch
          ></b-form-checkbox>
          </b-form-group>
          <b-form-group
          id="form-rating-group"
          label="Recipe Rating:"
          label-for="form-rating-input"
          description="Rate this recipe from 1-5"
          >
          <b-form-rating
          id="form-rating-input"
          type="integer"
          v-model="updateRecipeForm.rating"
          ></b-form-rating>
          </b-form-group>
  
          <b-button type="submit" variant="primary">Update</b-button>
          </b-form>
          </b-modal>
      <!-- End of Modal for Edit Account-->
    </div>
  </div>
</div>
</template>

<script>
import axios from "axios";
export default {
  name: "AppRecipes",
  data() {
    return {
      recipes: [],
      createRecipeForm: {
        name: "",
        ingredients: "",
        steps: "",
        rating: 1,
        favorite: false,
      },
      editRecipeForm: {
        id: "",
        name: "",
        ingredients: "",
        steps: "",
        rating: 1,
        favorite: false,
      },
      showMessage: false,
      message: "",
    };
  },
  methods: {
    /***************************************************
     * RESTful requests
     ***************************************************/
    //GET function
    RESTgetRecipes() {
      const path = `${process.env.VUE_APP_ROOT_URL}/`;
      axios
        .get(path)
        .then((response) => {
          this.recipes = response.data.recipes;
        })
        .catch((error) => {
          console.error(error);
        });
    },
    // POST function
    RESTcreateRecipe(payload) {
      const path = `${process.env.VUE_APP_ROOT_URL}/`;
      axios
        .post(path, payload)
        .then((response) => {
          this.RESTgetRecipes();
          // For message alert
          this.message = "Recipe Created succesfully!";
          // To actually show the message
          this.showMessage = true;
          // To hide the message after 3 seconds
          setTimeout(() => {
            this.showMessage = false;
          }, 3000);
        })
        .catch((error) => {
          console.error(error);
          this.RESTgetRecipes();
        });
    },
    // Update function
    RESTupdateRecipe(payload, recipeId) {
      const path = `${process.env.VUE_APP_ROOT_URL}/${recipeId}`;
      axios
        .put(path, payload)
        .then((response) => {
          this.RESTgetRecipes();
          // For message alert
          this.message = "Recipe Updated succesfully!";
          // To actually show the message
          this.showMessage = true;
          // To hide the message after 3 seconds
          setTimeout(() => {
            this.showMessage = false;
          }, 3000);
        });
    },
    // Delete account
    RESTdeleteRecipe(recipeId) {
      const path = `${process.env.VUE_APP_ROOT_URL}/${recipeId}`;
      axios
        .delete(path)
        .then((response) => {
          this.RESTgetRecipes();
          // For message alert
          this.message = "Recipe Deleted succesfully!";
          // To actually show the message
          this.showMessage = true;
          // To hide the message after 3 seconds
          setTimeout(() => {
            this.showMessage = false;
          }, 3000);
        })
        .catch((error) => {
          console.error(error);
          this.RESTgetRecipes();
        });
    },
    /***************************************************
     * FORM MANAGEMENT
     * *************************************************/
    // Initialize forms empty
    initForm() {
      this.createRecipeForm.name = "";
      this.createRecipeForm.ingredients = "";
      this.createRecipeForm.steps = "";
      this.createRecipeForm.rating = 1;
      this.createRecipeForm.favorite = false;
      this.editRecipeForm.name = "";
      this.editRecipeForm.ingredients = "";
      this.editRecipeForm.steps = "";
      this.editRecipeForm.rating = 1;
      this.editRecipeForm.favorite = false;
    },
    // Handle submit event for create recipe
    onSubmit(e) {
      e.preventDefault(); //prevent default form submit form the browser
      this.$refs.addRecipeModal.hide(); //hide the modal when submitted
      const payload = {
        name: this.createRecipeForm.name,
        ingredients: this.createRecipeForm.ingredients,
        steps: this.createRecipeForm.steps,
        rating: this.createRecipeForm.rating,
        favorite: this.createRecipeForm.favorite,
      };
      this.RESTcreateRecipe(payload);
      this.initForm();
    },
    // Handle submit event for edit recipe
    onSubmitUpdate(e) {
      e.preventDefault(); //prevent default form submit form the browser
      this.$refs.editRecipeModal.hide(); //hide the modal when submitted
      const payload = {
        name: this.editRecipeForm.name,
        ingredients: this.editRecipeForm.ingredients,
        steps: this.editRecipeForm.steps,
        rating: this.editRecipeForm.rating,
        favorite: this.editRecipeForm.favorite,
      };
      this.RESTupdateRecipe(payload, this.editRecipeForm.id);
      this.initForm();
    },
    // Handle edit button
    editRecipe(recipe) {
      this.editRecipeForm = recipe;
    },
    // Handle Delete button
    deleteRecipe(recipe) {
      this.RESTdeleteRecipe(recipe.id);
    },
  },
  /***************************************************
   * LIFECYClE HOOKS
   ***************************************************/
  created() {
    this.RESTgetRecipes();
  },
};
</script>