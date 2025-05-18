<template>
  <div class="ContactsHolst" ref="contactsHolst">
    <div class="ContactsBlock section" id="contacts-section">

      <div class="ContactsContentBlock">

        <div class="Email Block">
          <div class="Name oswald-text">Почта</div>
          <div class="Contacts">
            <a href="mailto:dikikh2006@gmail.com?subject=Заявка на создание сайта&body=Здравствуйте, ..." target="_blank" class="contacts-link oswald-text">dikikh2006@gmail.com</a>
          </div>
        </div>
        
        <div class="Phone Block">
          <div class="Name oswald-text">Телефон</div>
          <div class="Contacts">
            <a href="tel:+79145982842" target="_blank" class="contacts-link oswald-text">8 (914) 598-28-42</a>
          </div>
        </div>
        
        <div class="Social Block">
          <div class="Name oswald-text">Социальные сети</div>
          <div class="Contacts">
            <a href="https://vk.com/krovan06" target="_blank" class="contacts-link oswald-text">VK</a>
            <a href="tg://resolve?domain=krovan06" target="_blank" class="contacts-link oswald-text">Telegram</a>
            <a href="https://wa.me/89145982842" target="_blank" class="contacts-link oswald-text">Whatsapp</a>
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
  height: 80vh;
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
  height: 65%;
  display: flex;
  align-items: center;
  gap: 5vw;
  display: flex
}

.Block {
  height: 80%;
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.Name {
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: left;
  font-size: 23px;
}

.Contacts {
  width: 100%;
}

.contacts-link {
  width: 100%;
  text-decoration: none;
  color: rgb(97, 97, 97);
  display: flex;
  font-size: 18px;
  justify-content: left;
  flex-direction: column;
  transition: 0.3s;
}

.contacts-link:hover {
  color: rgb(21, 23, 27);
}

.AvtorName {
  display: flex;
  height: 50vh;
  justify-content: center;
  margin-top: 60vh;
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

@media (max-width: 700px) {

  .ContactsContentBlock {
  align-items: center;
  flex-direction: column;
  gap: 2vw;
  display: flex
}

  .ContactsBlock {
  width: 100vw;
  border-radius: 25px;
  height: 99%;
}

.Name {
  justify-content: center;
}

.contacts-link {
  font-size: 15px;
  align-items: center;
  flex-direction: column;
}

  .AvtorName {
  margin-top: 112vh;
}

.AvtorText {
  font-size: calc((1vh + 1vw) * 8);
  letter-spacing: calc((1vh + 1vw) * 1.2);
}
}
</style>