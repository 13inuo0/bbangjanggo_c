<template>
  <div class="login-wrap">
    <FlowAd />
    <form @submit.prevent="login" class="login-info-wrap">
      <h2>로그인</h2>
      <div class="login-input">
        <input v-model="usermail" type="email" placeholder="아이디 입력" />
        <input v-model="password" type="password" placeholder="비밀번호 입력" minlength="8" maxlength="12" />
      </div>
      <div class="login-box">
        <div class="logBtn">
          <button class="login-btn" type="submit">로그인</button>
          <router-link to="/signup" class="login-btn" type="button">회원가입</router-link>
        </div>

        <div class="login-help">
          <div class="loginHelp-top">
            <router-link to="/searchid">아이디 찾기</router-link>
            <div class="logline"></div>
            <router-link to="/searchPass">비밀번호 찾기</router-link>
            <div class="logline"></div>
            <router-link to="/loginworker">관리자 로그인</router-link>
          </div>
        </div>
        <div class="line"></div>
        <div class="sns-login">
          <p>간편 로그인</p>
          <div class="sns-login">
            <div class="sns-icon">
              <a href="#"><img src="/public/images/kms/sns-kakao.png" alt="카카오톡" /></a>
              <a href="#"><img src="/public/images/kms/sns-google.png" alt="구글" /></a>
              <a href="#"><img src="/public/images/kms/sns-naver.png" alt="네이버" /></a>
            </div>
          </div>
        </div>
      </div>
    </form>
  </div>
</template>
<script setup>
import FlowAd from "@/components/FlowAd.vue";
import { ref } from "vue";
import { useRouter } from "vue-router";

const router = useRouter();

const usermail = ref("");
const password = ref("");

const login = () => {
  const saved = localStorage.getItem("user");
  if (!saved) {
    alert("✨회원가입을 먼저 해주세요.");
    router.push("/signup");
    return;
  }
  const user = JSON.parse(saved);
  if (usermail.value === user.usermail && password.value === user.password) {
    alert("로그인 성공!");
    // 로그인 성공 했을 때 저장용 (로그인/로그아웃 제어)
    localStorage.setItem("loggedInUser", JSON.stringify(user));

    // ✅ 헤더가 같은 탭에서도 로그인 상태 변화를 감지할 수 있도록 이벤트 발송
    window.dispatchEvent(new Event("storage"));

    router.push("/");
  } else {
    alert("❌ 아이디 또는 비밀번호가 틀렸습니다.");
  }
};
</script>

<style lang="scss" scoped>
@use "/src/assets/variables" as *;
@use "/src/assets/btn" as *;

.login-wrap {
  background-color: $bg-color;
  height: calc(100vh - 89.77px - 115px);
  @media (max-width: 650px) {
    height: auto;
  }
  display: flex;
  flex-direction: column;
  align-items: center;

  .login-info-wrap {
    width: 100%;
    max-width: 1000px;
    margin: auto;
    @media (max-width: 650px) {
      padding: 50px 0;
    }
    h2 {
      font-family: "Cafe24Surround";
      color: $point-color;
      font-size: $sub-font;
      text-align: center;
      margin-bottom: 40px;
    }
    .login-input {
      display: flex;
      flex-direction: column;
      margin: auto;
      width: 90%;
      max-width: 400px;

      input {
        background-color: #fff;
        box-shadow: 1px 1px 4px rgba(0, 0, 0, 0.1);
        border: none;
        border-radius: 8px;
        padding: 20px 30px;
        margin-bottom: 20px;
      }
    }
    .login-box {
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 25px;
      color: $font-color;
      .line {
        height: 1px;
        width: 100%;
        max-width: 400px;
        background-color: #ccc;
        margin: 3.5px 0;
      }
      .sns-login {
        display: flex;
        flex-direction: column;
        align-items: center;
        margin: auto;
        max-width: 400px;
        gap: 20px;
        @media (max-width: 768px) {
          p {
            font-size: $notice-text-font;
          }
        }

        .sns-icon {
          width: 100%;
          display: flex;
          justify-content: space-between;
          img {
            display: block;
            width: 120px;
            padding: 0 30%;
          }
        }
      }
      .login-help {
        display: flex;
        gap: 25px;
        align-items: center;
        justify-content: center;
        width: 100px;
        white-space: nowrap;
        .loginHelp-top {
          display: flex;
          gap: 10px;
          .logline {
            height: 15px;
            width: 1px;
            background-color: #ccc;
            margin: 3px;
          }
        }
      }
      .logBtn {
        display: flex;
        flex-direction: column;
        width: 90%;
        max-width: 400px;
        align-items: center;
      }
      .login-btn {
        @include btn-style;
        max-width: 500px;
        text-align: center;
        padding: 20px 30px;
        margin: 20px 0 0 0;
        font-size: 14px;
        &:nth-child(2) {
          background-color: $main-color;
          color: $font-color;
        }
      }
      @media (max-width: 768px) {
        .login-btn {
          font-size: $notice-text-font;
        }
      }
    }
  }
}

// 반응형
@media (max-width: 768px) {
  .login-help {
    display: flex;
    flex-direction: column;
    font-size: $mobile-text-font;
    .loginHelp-top {
      gap: 5px;
    }
  }
}
@media (max-width: 320px) {
  .login-help {
    display: flex;
    flex-direction: column;
    font-size: $mobile-notice-font;
    .loginHelp-top {
      gap: 3px;
    }
  }
  .logBtn {
    font-size: $mobile-notice-font;
  }
}
</style>
