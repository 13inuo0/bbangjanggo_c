<template>
  <div class="mypage">
    <div class="inner mypage_wrap">
      <h1>마이페이지</h1>
      <h3 class="hello">안녕하세요 김빵장님!</h3>
      <div class="web_page">
        <div class="mypage-box">
          <div class="reser-txt">
            <div class="reservation">예약내역</div>
            <div class="boso">
              <ul class="locker">
                <li class="sub_list">
                  <h4>보관소</h4>
                  <p class="store">반월당역점</p>
                </li>
                <li class="sub_list">
                  <h4>방문일시</h4>
                  <p>25.09.23 [11:00~11:30]</p>
                </li>
                <li class="sub_list">
                  <h4>방문빵집</h4>
                  <p class="bread">빵순이네</p>
                </li>
              </ul>
              <ul class="methodlocker">
                <li class="sub_list">
                  <h4>보관방법</h4>
                  <p class="store">기사님께 맡길게요</p>
                </li>
                <li class="sub_list">
                  <h4>온도</h4>
                  <p>상온보관</p>
                </li>

                <li class="sub_list">
                  <h4>부가서비스</h4>
                  <p class="bread">선택안함</p>
                </li>
                <button class="reser_change">예약 변경 및 취소</button>
              </ul>
            </div>

            <div class="btn">
              <button class="stemp">스탬프</button>
              <button class="review">리뷰관리</button>
            </div>
          </div>

          <ul class="reser-info">
            <li>회원정보</li>
            <li>이벤트</li>
            <li>공지사항</li>
            <li>자주묻는질문</li>
            <li>고객센터</li>
            <li>약관 및 정책</li>
          </ul>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup></script>

<style lang="scss" scoped>
@use "/src/assets/variables" as *;

.mypage {
  // height: calc(100vh - 290px);
  height: calc(100vh - 89.77px - 115px);
  @media (max-width: 650px) {
    height: auto;
  }
  display: flex;
flex-direction: column;
align-items: center;
  background-color: $bg-color;
  // padding-bottom: 50px;
  position: relative;

  // .mypage_wrap {
  //   position: absolute;
  //   top: 50%;
  //   left: 50%;
  //   transform: translate(-50%, -50%);
  // }
}
h1 {
  text-align: center;
  padding: 0 0 50px;
  font-family: "Cafe24Surround";
  color: $point-color;
  font-size: 35px;
}
h3 {
  font-size: $sub-font;
  font-weight: 700;
  padding-bottom: 30px;
  color: $font-color;
}

.mypage-box {
  display: flex;
  justify-content: space-between;
  gap: 30px;
}
.reservation {
  color: $font-color;
  font-size: $sub-font;
  font-weight: bold;
  padding-bottom: 30px;
}

.reser-txt {
  width: 60%;
  padding: 30px;
  background-color: #fff;
  box-shadow: 1px 1px 6px rgba(0, 0, 0, 0.1);
  border-radius: 15px;
}
.reser-info {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  width: 40%;
  font-size: 12px;
  background-color: #fff;
  box-shadow: 1px 1px 6px rgba(0, 0, 0, 0.1);
  border-radius: 15px;
  padding: 30px;
  font-family: "SpokaHanSansNeo";
}
.reser-info li {
  // padding-bottom: 25px;
  font-family: "SpokaHanSansNeo";
  color: $font-color;
  font-size: $desc-text-font;
  &:last-child {
    padding-bottom: 0;
  }
}
.boso {
  width: 100%;
  display: flex;
  justify-content: space-between;
  font-size: 12px;
  gap: 5%;
}
.boso ul {
  width: calc(100% / 2);
}
.boso ul li {
  padding-bottom: 10%;
}
.boso ul li span {
  padding-right: 0;
}

.sub_list {
  display: flex;
  gap: 5px;
  padding-bottom: 15px;
  font-family: "SpokaHanSansNeo";
}

h4 {
  font-size: 14px;
  text-align: left;
  width: 40%;
}

.methodlocker p {
  margin-right: 10px;
  // font-size: 16px;
}

.btn {
  display: flex;
  gap: 25px;
  margin-top: 30px;
}
.btn button {
  background-color: #ffebc2;
  border-radius: 8px;

  width: 100%; /* 부모 폭에 맞추기 */
  border: none;
  font-family: "SpokaHanSansNeo";
  color: $font-color;
}
button.stemp,button.review
/* {  width: 286px;
  height: 70px; */ {
  padding: 2%;
  font-family: "SpokaHanSansNeo";
  font-size: $desc-text-font;
  font-weight: 600;
  cursor: pointer;
}
button.reser_change {
  border: none;

  background-color: #ffffff;
  text-align: end;
  cursor: pointer;
  font-size: 11px;
  text-decoration: underline;
  width: 100%;
}

@media screen and (max-width: 768px) {
  .mypage {
    background-color: $bg-color;
    height: auto;
    padding: 8% 0;
  }

  .box_bo {
    background-color: #fff;
    border-radius: 18px;
    // width: 329px;
  }

  .mypage-box {
    flex-wrap: wrap;
  }
  .reser-txt {
    width: 100%;
  }
  .reser-info {
    // margin-top: 30px;
    width: 100%;
    gap: 25px;
  }
}
@media screen and (max-width: 390px) {
  .mypage-box {
    gap: 25px;
  }
  h1 {
    font-size: $title-font;
    padding: 0 0 30px;
  }

  h3,
  .reservation {
    font-size: $f-a-q-text-font;
  }
  .btn {
    gap: 10px;
  }
  // .boso {
  //   gap: 5px;
  // }
  .reser-txt {
    padding: 25px;
  }
  .sub_list {
    display: flex;
    align-items: center; /* h4와 p를 세로 중앙 정렬 */
    gap: 8px;
    width: 100%;
    padding-bottom: 10px;
    h4 {
      flex: 0 0 70px;
      font-size: $mobile-notice-font;
      text-align: left;
      // width: 70px;
    }
    p {
      flex: 1;
      font-size: 11px;
      line-height: 1.4;
      word-break: keep-all; /* 단어 단위로 줄바꿈 */
    }
  }
  .reser-info {
    padding: 25px;
    li {
      font-size: $notice-text-font;
    }
  }
}
</style>
