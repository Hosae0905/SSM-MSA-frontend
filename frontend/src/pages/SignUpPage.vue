<template>
  <div class="container">
    <div class="card o-hidden border-0 shadow-lg my-5">
      <div class="card-body p-0">
        <!-- Nested Row within Card Body -->
        <div class="row">
          <div class="col-lg-7">
            <div class="p-5">
              <div class="text-center">
                <h1 class="h4 text-gray-900 mb-4">회원 가입</h1>
              </div>
              <form class="user" enctype="multipart/form-data">
                <div class="form-group">
                  <div class="input-group">
                    <input v-model="memberStore.member.memberEmail" type="text" class="form-control form-control-user"
                      placeholder="아이디를 입력해주세요" :disabled="memberStore.checkId" >
                    <div class="input-group-append">
                      <button type="button" class="btn btn-primary btn-user btn-block" @click="checkIdDuplicate">중복
                        검사</button>
                    </div>
                  </div>
                  <span class="red-text" v-show="memberStore.checkId">
                    아이디 확인이 완료 됐습니다.
                  </span>
                </div>
                <div class="form-group">
                  <input v-model="memberStore.member.name" type="text" class="form-control form-control-user"
                    placeholder="이름을 입력해주세요">
                </div>
                <div class="form-group">
                  <input v-model="memberStore.member.memberPw" type="password" class="form-control form-control-user"
                    placeholder="비밀번호를 입력해주세요">
                </div>
                <div class="form-group">
                  <input v-model="memberStore.member.memberPwChecked" type="password"
                    class="form-control form-control-user" placeholder="비밀번호를 한번더 입력해주세요">
                </div>
                <div class="form-group">
                  <input v-model="memberStore.member.department" type="text" class="form-control form-control-user"
                    placeholder="부서명을 입력해주세요">
                </div>
                <div class="form-group">
                  <input v-model="memberStore.member.position" type="text" class="form-control form-control-user"
                    placeholder="직책을 입력해주세요">
                </div>
                <div class="form-group">
                  <input type="file" id="fileUpload" @change="handleFileUpload" accept="image/*" style="display: none;">
                  <label for="fileUpload" class="btn btn-primary btn-user btn-block">프로필 이미지 선택</label>
                  <span id="fileName">선택된 파일 없음</span>
                </div>
                <div id="imageContainer">
                  <img id="imagePreview" src="" alt="이미지 미리보기" style="max-width: 100%; height: auto; display: none;">
                </div>
                <hr>
              </form>
              <button @click="signup" class="btn btn-primary btn-user btn-block">
                회원 가입
              </button>
              <hr>
              <div class="text-center">
                <a class="small" href="/login">이미 계정이 있으신가요? 로그인!</a>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { mapStores } from "pinia";
import { useMemberStore } from "@/stores/useMemberStore";

export default {
  name: 'SignUpPage',
  data() {
    return {

    }
  }, computed: {
    ...mapStores(useMemberStore),
  },
  methods: {
    signup() {
      this.memberStore.signup(this.$router);
    },
    checkIdDuplicate() {
      this.memberStore.checkIdDuplicate(this.$router);
    },
    handleFileUpload(event) {
      const file = event.target.files[0];
      this.memberStore.member.profileImage = file;
      document.getElementById('fileName').textContent = file ? file.name : '선택된 파일 없음';

      if (file && file.type.startsWith('image/')) {
        const reader = new FileReader();

        reader.onload = (e) => {
          const imagePreview = document.getElementById('imagePreview');
          imagePreview.src = e.target.result;
          imagePreview.style.display = 'block';
          imagePreview.style.width = '200px';
          imagePreview.style.height = '200px';
        };

        reader.readAsDataURL(file);
      }
    },
  }
}
</script>

<style scoped>
.red-text {
    color: red;
  }
  
.disabled-input {
  background-color: #e0e0e0; /* 회색 */
  color: #a0a0a0; /* 입력된 텍스트의 색깔도 변경하고 싶다면 추가 */
}

.input-group {
  display: flex;
}

.input-group-append {
  margin-left: 8px;
  /* 버튼과 인풋 필드 사이의 간격 조정 */
}

.control-label {
  display: block;
  margin-bottom: 5px;
  /* 라벨과 인풋 필드 사이의 간격 조정 */
}

/* 버튼 크기가 인풋 필드와 다를 경우 */
.btn-secondary {
  height: calc(1.5em + .75rem + 2px);
  /* 인풋 필드와 높이를 맞추기 */
  margin-bottom: 0;
  /* 인풋 필드와 버튼의 하단 정렬을 위해 */
}

#imageContainer {
  display: flex;
  /* Flexbox 레이아웃 사용 */
  justify-content: center;
  /* 가로 방향 가운데 정렬 */
  align-items: center;
  /* 세로 방향 가운데 정렬 */
  height: auto;
  /* 컨테이너의 높이 설정, 필요에 따라 조정 */
}

* {
  box-sizing: border-box;
}

.container,
.container-lg,
.container-md,
.container-xl {
  width: 100%;
  padding-right: .75rem;
  padding-left: .75rem;
  margin-right: auto;
  margin-left: auto;
}

@media (min-width:576px) {
  .container {
    max-width: 540px;
  }
}

@media (min-width:768px) {

  .container,
  .container-md {
    max-width: 720px;
  }
}

@media (min-width:992px) {

  .container,
  .container-lg,
  .container-md {
    max-width: 960px;
  }
}

@media (min-width:1200px) {

  .container,
  .container-lg,
  .container-md,
  .container-xl {
    max-width: 1140px;
  }
}

.row {
  display: flex;
  text-align: center;
  flex-wrap: wrap;
  margin-right: -.75rem;
  margin-left: -.75rem;
}

.justify-content-center {
  justify-content: center !important;
}

.col,
.col-1,
.col-10,
.col-11,
.col-12,
.col-2,
.col-3,
.col-4,
.col-5,
.col-6,
.col-7,
.col-8,
.col-9,
.col-lg,
.col-lg-1,
.col-lg-10,
.col-lg-11,
.col-lg-12,
.col-lg-2,
.col-lg-3,
.col-lg-4,
.col-lg-5,
.col-lg-6,
.col-lg-7,
.col-lg-8,
.col-lg-9,
.col-md,
.col-md-1,
.col-md-10,
.col-md-11,
.col-md-12,
.col-md-2,
.col-md-3,
.col-md-4,
.col-md-5,
.col-md-6,
.col-md-7,
.col-md-8,
.col-md-9,
.col-xl,
.col-xl-1,
.col-xl-10,
.col-xl-11,
.col-xl-12,
.col-xl-2,
.col-xl-3,
.col-xl-4,
.col-xl-5,
.col-xl-6,
.col-xl-7,
.col-xl-8,
.col-xl-9 {
  position: relative;
  width: 100%;
  padding-right: .75rem;
  padding-left: .75rem;
}

.card {
  position: relative;
  display: flex;
  flex-direction: column;
  min-width: 0;
  word-wrap: break-word;
  background-color: #fff;
  background-clip: border-box;
  border: 1px solid #e3e6f0;
  border-radius: .35rem;
}

.o-hidden {
  overflow: hidden !important;
}

.border-0 {
  border: 0 !important;
}

.shadow-lg {
  box-shadow: 0 1rem 3rem rgba(0, 0, 0, .175) !important;
}

.my-5 {
  margin-top: 3rem !important;
  margin-bottom: 3rem !important;
}

.card-body {
  flex: 1 1 auto;
  min-height: 1px;
  /* 기존 설정 */
  padding: 1.25rem;
  
}

.p-0 {
  padding: 0 !important;
}

.p-5 {
  padding: 2.5rem !important;
}

.text-center {
  text-align: center !important;
}

h1,
h2,
h3,
h4,
h5,
h6 {
  margin-top: 0;
  margin-bottom: .5rem;
}

.text-gray-900 {
  color: #3a3b45 !important;
}

.mb-4,
.my-4 {
  margin-bottom: 1.5rem !important;
}

.form-group {
  margin-bottom: 1rem;
}

.form-control {
  display: block;
  width: 100%;
  height: calc(1.5em + .75rem + 2px);
  padding: .375rem .75rem;
  font-size: 1rem;
  font-weight: 400;
  line-height: 1.5;
  color: #6e707e;
  background-color: #fff;
  background-clip: padding-box;
  border: 1px solid #d1d3e2;
  border-radius: .35rem;
  transition: border-color .15s ease-in-out, box-shadow .15s ease-in-out;
}

form.user .form-control-user {
  font-size: .8rem;
  border-radius: 10rem;
  padding: 1.5rem 1rem;
}

.custom-control {
  align-items: center;
  justify-content: center;
}

.custom-checkbox .custom-control-label::before {
  border-radius: .35rem;
}

small {
  font-size: 80%;
}

.small,
small {
  font-size: 80%;
  font-weight: 400;
}

.btn {
  display: inline-block;
  font-weight: 400;
  color: #858796;
  text-align: center;
  vertical-align: middle;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
  background-color: transparent;
  border: 1px solid transparent;
  padding: .375rem .75rem;
  font-size: 1rem;
  line-height: 1.5;
  border-radius: .35rem;
  transition: color .15s ease-in-out, background-color .15s ease-in-out, border-color .15s ease-in-out, box-shadow .15s ease-in-out;
}

.btn-primary {
  color: #fff;
  background-color: #4e73df;
  border-color: #4e73df;
}

.btn-primary:hover {
  color: #fff;
  background-color: #2e59d9;
  border-color: #2653d4;
}

.btn-primary:focus {
  color: #fff;
  background-color: #2e59d9;
  border-color: #2653d4;
  box-shadow: 0 0 0 .2rem rgba(105, 136, 228, .5);
}

form.user .btn-user {
  font-size: .8rem;
  border-radius: 10rem;
  padding: .75rem 1rem;
}

.btn-block {
  display: block;
  width: 100%;
}

.btn-google {
  color: #fff;
  background-color: #ea4335;
  border-color: #fff;
}

.btn-google:hover {
  color: #fff;
  background-color: #e12717;
  border-color: #e6e6e6;
}

.btn-google:focus {
  color: #fff;
  background-color: #e12717;
  border-color: #e6e6e6;
  box-shadow: 0 0 0 .2rem rgba(255, 255, 255, .5);
}
</style>