<template>
  <div class="home">
    <div class="app-phone">
      <div class="phone-header">
        <img src="@/assets/vuestagram.png">
        <a class="cancel-cta" v-if="step === 2 || step === 3" @click="goToHome">Cancel</a>
        <a class="next-cta" v-if="step === 2" @click="this.$store.commit('setStep', 3)">Next</a>
        <a class="next-cta" v-if="step === 3" @click="sharePost">Share</a>
      </div>
      <phone-body
        v-model="caption"
      />
      <div class="phone-footer">
        <div class="home-cta" @click="goToHome">
          <i class="fas fa-home fa-lg"></i>
        </div>
        <button class="logout-button" @click="logout">ログアウト</button>
        <div class="upload-cta">
          <input
            type="file"
            name="file"
            id="file"
            class="inputfile"
            @change="uploadImage"
            :disabled="step !== 1"
          >
          <label for="file">
            <i class="far fa-plus-square fa-lg"></i>
          </label>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import PhoneBody from "@/components/PhoneBody";
export default {
  name: 'Home',
  components: {
    "phone-body": PhoneBody
  },
  computed: {
    step(){
      return this.$store.getters.getStep;
    },
    caption(){
      return this.$store.getters.getInputCaption;
    }
  },
  created() {
    this.$store.dispatch('initPostData');
  },
  methods: {
    goToHome() {
      this.$store.dispatch('resetStepAction');
    },
    uploadImage(evt) {
      const files = evt.target.files;
      if (!files.length) return;
      const reader = new FileReader();
      reader.readAsDataURL(files[0]);
      reader.onload = evt => {
        this.$store.commit('setUploadImage', evt.target.result);
        this.$store.commit('setStep', 2);
      };
      // To enable reuploading of same files in Chrome
      document.querySelector("#file").value = "";
    },
    sharePost() {
      this.$store.dispatch('sharePostAction');
      this.goToHome();
    },
    logout(){
      this.$store.dispatch('logout');
      this.$router.push('/auth');
    }
  }
}
</script>

<style>
.app-phone {
  background-color: white;
  height: 620px;
  width: 375px;
  overflow: hidden;
}

.phone-header {
  height: auto;
  width: 375px;
  position: sticky;
  position: -webkit-sticky;
  top: 0;
  background: #fafafa;
  border-bottom: 1px solid #eeeeee;
  z-index: 99;
}
.phone-header img {
  max-width: 120px;
  display: block;
  margin: 0 auto;
  padding-top: 1px;
}
.phone-header .cancel-cta,
.phone-header .next-cta {
  position: absolute;
  top: 12px;
  color: #209cee;
  font-weight: bold;
  cursor: pointer;
}
.phone-header .cancel-cta {
  left: 10px;
}
.phone-header .next-cta {
  right: 10px;
}

.feed {
  height: 100%;
  overflow-y: scroll;
  overflow-x: hidden;
  margin-right: -15px;
}

.caption-container {
  height: 210px;
  display: flex;
  align-items: center;
  justify-content: center;
}
.caption-container textarea {
  border: 0;
  font-size: 1rem;
  width: 100%;
  padding: 10px;
  border-bottom: 1px solid #eeeeee;
}
.caption-container textarea:focus {
  outline: 0;
}

.selected-image {
  background-repeat: no-repeat;
  background-size: cover;
  background-position: center center;
  height: 330px;
}

.phone-footer {
  height: 35px;
  width: 375px;
  position: sticky;
  position: -webkit-sticky;
  bottom: 0;
  background: #fafafa;
  border-top: 1px solid #eeeeee;
  z-index: 99;
}
.phone-footer .home-cta {
  position: absolute;
  left: 10px;
  top: 6px;
  width: 30px;
  cursor: pointer;
}
.phone-footer .logout-button {
  position: absolute;
  left: 50px;
  top: 6px;
  cursor: pointer;
  z-index: 10;
}
.phone-footer .upload-cta {
  position: absolute;
  right: 10px;
  top: 6px;
}
.phone-footer .upload-cta p {
  font-size: 0.63rem;
  position: absolute;
  left: -25px;
  top: 5px;
}
.phone-footer input[name=file] {
  visibility: hidden;
}
.phone-footer label {
  cursor: pointer;
  z-index: 99;
}

.fade-leave-active {
  transition: opacity 0.5s;
}

.fade-leave-to {
  opacity: 0;
}

@media (max-width: 520px) {
  .app-phone, .app-phone-scroll-cover {
    height: 100%;
    width: 100%;
  }

  .phone-header, .phone-footer {
    width: 100%;
  }
}
@media (max-height: 520px) {
  .app-phone {
    transform: scale(0.6);
  }
}
</style>