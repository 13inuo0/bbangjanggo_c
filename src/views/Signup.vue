<template>
  <FlowAd />
  <!-- 회원가입 전체 -->
  <div class="signup">
    <div class="signup-inner">
      <!-- 타이틀 -->
      <h2>회원가입</h2>
      <!-- 회원가입 정보 입력란 -->
      <form class="signupInfo" @submit.prevent="signup">
        <div class="infoBox">
          <p>성함</p>
          <input v-model="username" type="text" placeholder="한글 또는 영어 이름을 입력해주세요" />
        </div>
        <div class="infoBox-a">
          <p>휴대폰 번호</p>
          <div class="info-txt-box">
            <input v-model="usernumber" @input="formatPhone" maxlength="13" type="text" placeholder="010-1234-5678" />
            <button :disabled="isButtonDisabled" @click="sendAuthCode" type="button">
              {{ isButtonDisabled ? `재전송 (${timer}s)` : `인증번호` }}
            </button>
          </div>
        </div>
        <div class="infoBox-a">
          <p>인증번호 입력</p>
          <div class="info-txt-box">
            <input v-model="authCode" type="text" placeholder="인증번호 6자리 입력" maxlength="6" />
            <button @click="verifyAuthCode" type="button">인증확인</button>
          </div>
        </div>
        <!-- 인증번호 관련 상태 알림말  -->
        <p v-if="authMessage" :class="{ success: isVerified, error: !isVerified }" class="auth-txt">
          {{ authMessage }}
        </p>
        <div class="infoBox">
          <p>이메일</p>
          <input v-model="usermail" type="email" placeholder="you@email.com" />
        </div>
        <div class="infoBox">
          <p>비밀번호</p>
          <input
            v-model="password"
            type="password"
            placeholder="8자 이상, 영문자 + 숫자 포함"
            minlength="8"
            maxlength="12"
          />
        </div>
        <div class="infoBox">
          <p>비밀번호 확인</p>
          <input
            v-model="passwordConfirm"
            @input="checkPasswordMatch"
            type="password"
            placeholder="비밀번호를 재입력해주세요"
            minlength="8"
            maxlength="12"
          />
        </div>
        <p v-if="passwordMessage" :class="{ success: isPasswordMatch, error: !isPasswordMatch }">
          {{ passwordMessage }}
        </p>
        <SignupTerms ref="termsRef" />

        <!-- 회원가입 버튼 -->
        <button type="submit" class="signupBtn">회원 가입</button>
      </form>
    </div>
  </div>
</template>
<script setup>
import FlowAd from "@/components/FlowAd.vue";
import SignupTerms from "@/components/SignupTerms.vue";
import { ref } from "vue";
import { useRouter } from "vue-router";

const router = useRouter();

const username = ref(""); //이름
const password = ref(""); //비밀번호
const passwordConfirm = ref(""); //비밀번호 확인
const usermail = ref(""); //이메일
const usernumber = ref(""); //전화번호
const authCode = ref(""); //인증코드

// 약관 컴포넌트 ref
const termsRef = ref(null);

// 인증코드 관련 ref
const timer = ref(0); //타이머 표시
const isButtonDisabled = ref(false); //인증코드 버튼 가리기
const isVerified = ref(false);
const authMessage = ref(""); //인증코드 관련 알림창

// 비밀번호 확인 관련
const isPasswordMatch = ref(false); //비밀번호 일치 여부
const passwordMessage = ref(""); //비밀번호 확인 메시지

// 📞전화번호 -(하이픈) 적용하기
const formatPhone = () => {
  // 숫자만 남기기
  let num = usernumber.value.replace(/[^0-9]/g, "");

  // 형식 적용
  if (num.length < 4) {
    usernumber.value = num;
  } else if (num.length < 8) {
    usernumber.value = `${num.slice(0, 3)}-${num.slice(3)}`;
  } else {
    usernumber.value = `${num.slice(0, 3)}-${num.slice(3, 7)}-${num.slice(7, 11)}`;
  }
};

// 비밀번호 재확인 일치 확인
const checkPasswordMatch = () => {
  if (!passwordConfirm.value) {
    passwordMessage.value = "";
    isPasswordMatch.value = false;
    return;
  }

  if (password.value === passwordConfirm.value) {
    isPasswordMatch.value = true;
    passwordMessage.value = "비밀번호가 일치합니다 ✅";
  } else {
    isPasswordMatch.value = false;
    passwordMessage.value = "비밀번호가 일치하지 않습니다 ❌";
  }
};

// ✨📲인증번호 전송 (요청 버튼 클릭 시)
const sendAuthCode = () => {
  // 전화번호 입력 확인
  if (!usernumber.value || usernumber.value.replace(/[^0-9]/g, "").length !== 11) {
    alert("올바른 전화번호를 입력해주세요.");
    return;
  }

  // 예: 서버에 전화번호 전송 후 인증번호 발송 API 요청
  console.log("인증번호 요청됨");

  // 버튼 비활성화 + 타이머 시작
  isButtonDisabled.value = true;
  timer.value = 60;

  const countdown = setInterval(() => {
    timer.value--;
    if (timer.value <= 0) {
      clearInterval(countdown);
      isButtonDisabled.value = false;
    }
  }, 1000);
};

// ✅ 인증번호 검증
const verifyAuthCode = () => {
  if (!authCode.value) {
    alert("인증번호를 입력해주세요.");
    return;
  }

  // 실제로는 서버 검증 API 호출
  if (authCode.value === "123456") {
    isVerified.value = true;
    authMessage.value = "인증이 완료되었습니다 ✅";
  } else {
    isVerified.value = false;
    authMessage.value = "인증번호가 일치하지 않습니다 ❌";
  }
};

// 회원가입 완료
const signup = () => {
  // 2️⃣ 비밀번호 일치 확인
  if (!isPasswordMatch.value) {
    alert("비밀번호가 일치하지 않습니다.");
    return;
  }

  // 3️⃣ 필수 약관 동의 확인
  if (!termsRef.value?.isRequiredTermsChecked) {
    alert("필수 약관에 모두 동의해주세요.");
    return;
  }

  // 4️⃣ 모든 검증 통과 시 회원가입 진행
  const user = {
    username: username.value,
    password: password.value,
    usermail: usermail.value,
    usernumber: usernumber.value,
  };

  localStorage.setItem("user", JSON.stringify(user));
  alert("회원가입 완료");
  router.push("/login");
};
</script>

<style lang="scss" scoped>
@use "/src/assets/variables" as *;
@use "/src/assets/btn" as *;

.signup {
  background-color: $bg-color;
  .signup-inner {
    width: 85%;
    max-width: 500px;
    margin: auto;
    padding-top: 50px;

    h2 {
      font-family: "Cafe24Surround";
      color: $point-color;
      font-size: $sub-font;
      text-align: center;
      margin-bottom: 40px;
    }

    .signupInfo {
      .infoBox {
        p {
          font-size: $desc-text-font;
          color: $font-color;
          font-weight: bold;
          margin-bottom: 5px;
        }
        input {
          width: 100%;
          background-color: #fff;
          border: none;
          box-shadow: 1px 1px 4px rgba(0, 0, 0, 0.1);
          border-radius: 8px;
          padding: 20px 30px;
          margin-bottom: 20px;
        }
      }
      .infoBox-a {
        p {
          font-size: $desc-text-font;
          color: $font-color;
          font-weight: bold;
          margin-bottom: 10px;
        }
        .info-txt-box {
          display: flex;
          margin-bottom: 20px;
          gap: 10px;
          white-space: nowrap;
          input {
            width: 70%;
            background-color: #fff;
            border: none;
            box-shadow: 1px 1px 4px rgba(0, 0, 0, 0.1);
            border-radius: 8px;
            padding: 20px 30px;
          }
          button {
            width: 30%;
          }
        }
      }
    }
  }
  button {
    @include btn-style;
    height: 59px;
  }
}
// 회원가입 버튼
.signupBtn {
  margin: 25px 0;
}

//인증코드 관련 css
.auth-section {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

button:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

.success {
  color: rgb(63, 168, 63);
}
.error {
  color: rgb(245, 83, 83);
}

.auth-txt {
  margin-bottom: 15px;
  margin-left: 5px;
  font-size: $notice-text-font;
}
</style>
