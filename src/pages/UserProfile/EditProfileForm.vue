<template>
  <card>
    <h4 slot="header" class="card-title">Sensitive Content Filtering</h4>
    <form>

      <div class="row">
        <div class="col-md-12">
          <div class="form-group">
            <label>About Me</label>
            <textarea rows="5" class="form-control border-input" id="content"
                      placeholder="Here can be your description"
                      v-model="user.aboutMe" @input="sendText">
              </textarea>
          </div>
        </div>
      </div>
      <div class="text-center">
        <button type="submit" class="btn btn-info btn-fill float-right" @click.prevent="updateProfile">
          Predict
        </button>
      </div>

      <div class="row">
        <div class="col-md-12">
          <div class="form-group">
            <label>Results</label>
            <p> Origin: <span id = "origin"></span></p>
            <p> Sensitive: <span id = "sensitive"></span></p>
            <span></span>
            <div class="placeholder" contenteditable="false"> {{autoComplete}}</div>
          </div>
        </div>
      </div>
      <div class="clearfix"></div>
    </form>
  </card>

  
</template>
<script>
  import Card from 'src/components/Cards/Card.vue'
  import axios from 'axios'
  export default {
    components: {
      Card
    },
    data () {
      return {
        user: {
          company: 'Light dashboard',
          username: 'michael23',
          email: '',
          firstName: 'Mike',
          lastName: 'Andrew',
          address: 'Melbourne, Australia',
          city: 'melbourne',
          country: 'Australia',
          postalCode: '',
          aboutMe: `Xin chào`
        }
      }
    },
    methods: {
      async updateProfile (e) {
        // alert('Your data: ' + JSON.stringify(this.user))
        // console.log('Your data: ' + JSON.stringify(this.user))
        var content = document.getElementById("content").value
        // const formData = new FormData();
        // formData.append("text",content);

        await  axios
        .get("http://10.10.1.15:10020/predict-text/", {params: {
          text: content
        }})
        .then(async (res) => {
            console.log(res.data.Original )
            console.log(res.data.Sensitive)
            document.getElementById('origin').innerHTML =res.data.Original;
            document.getElementById('sensitive').innerHTML = (res.data.Sensitive ==0) ? "False" : "True";
        })
        .catch((err) =>console.log(err))
      }
    }
  }

</script>
<style>

</style>
