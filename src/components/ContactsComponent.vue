<template>
  <div class="ContactsHolst" ref="contactsHolst">
    <div class="ContactsBlock section" id="contacts-section">
      <div class="ContactsContentBlock">

        <div class="MessengersBlock">

          <div class="TestBlock">
            <div class="IconMessengers"></div>
          </div>

          <div class="TestBlock">
            <div class="IconMessengers"></div>
          </div>

          <div class="TestBlock">
            <div class="IconMessengers"></div>
          </div>


        </div>

      </div>
      <div class="AvtorName">
        <div class="AvtorText poppins-text">
          <div class="AvtorText poppins-text WordAnimation">
            DIKIKH
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.ContactsHolst {
  width: 100%;
  height: 100vh;
  background-color: black;
  position: relative;
  overflow: hidden;
}

.ContactsBlock {
  width: 100vw;
  border-radius: 25px;
  height: 90%;
  margin-top: 1vh;
  display: flex;
  align-items: center;
  justify-content: center;
  background-color: rgb(231, 244, 255);
}

.ContactsContentBlock {
  width: 85%;
  height: 90%;
  border: 1px solid black;
}

.MessengersBlock {
  width: 30%;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  gap: 10vh;
  border: 1px solid red;

  position: relative;
  z-index: 1;
}

.TestBlock {
  width: 60%;
  border: 1px solid green;
}

.IconMessengers {
  width: 90px;
  height: 90px;
  background-color: rgb(21, 23, 27);
  border-radius: 100%;
  transition: 0.1s;
}

.IconMessengers:hover {
  cursor: pointer;
  background-color: black
}

.IconMessengers:active {
  background-color: rgb(38, 38, 38);
}

.ButtonMessengers {
  margin-right: 10%;
  width: 60%;
  height: 60%;
  background-color: rgb(21, 23, 27);
  border-radius: 15px;
  transition: 0.2s;

  display: flex;
  align-items: center;
  justify-content: center;
  color: aliceblue;
  letter-spacing: 4px;
}

.ButtonAnimation {
  animation: buttonShow 1s forwards;
}

@keyframes buttonShow {
  0% {
    opacity: 0;
    width: 10%;
  }

  100% {
    opacity: 1;
    width: 60%;
  }
}

.ButtonMessengers:hover {
  cursor: pointer;
  color: rgb(21, 23, 27);
  background-color: aliceblue;
  border: 1px solid rgb(21, 23, 27);
}

.ButtonMessengers:active {
  height: 55%;
  background-color: rgb(151, 158, 164);
  border: 1px solid rgb(255, 255, 255);
}

.AvtorName {
  display: flex;
  height: 50vh;
  justify-content: center;
  margin-top: 70vh;
  position: absolute;
}

.AvtorText {
  display: flex;
  color: rgb(21, 23, 27);
  font-size: calc((1vh + 1vw) * 17);
  letter-spacing: calc((1vh + 1vw) * 1.5);
  transition: 3s;
}

.AvtorText:hover {
  cursor: default;
  color: rgb(32, 36, 45);
  /*
  text-shadow: 20px -8px 18px rgb(35, 35, 35);
  */
}

.poppins-text {
  font-family: 'Poppins', Arial, sans-serif;
  font-weight: 600;
  font-style: normal;
}

.WordAnimation {
  animation: wordAnimation 2s forwards;
}

.oswald-text {
  font-family: 'Oswald', sans-serif;
  font-weight: 500;
}

@keyframes wordAnimation {
  0% {
    margin-top: 12vh;
    opacity: 0;
    filter: blur(10px);
  }

  100% {
    margin-top: 0px;
    opacity: 1;
    filter: blur(0px);
  }
}
</style>

<script>
  export default {

  }
</script>