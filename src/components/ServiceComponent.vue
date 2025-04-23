<template>
  <div class="ServiceComponentHolst">
    <div class="ServiceContainer">
      <div class="ServiceInteractive">
        <div class="ServiceBlock">
          
        </div>
      </div>
      <div class="ServiceContentContainer section" id="services-section">
        <div class="Line">
          <div class="Service"><p class="ServiceText poppins-text">Бэкенд</p></div>
        </div>
        <div class="Line">
          <div class="Service"><p class="ServiceText poppins-text">Фронтенд</p></div>
        </div>
        <div class="Line">
          <div class="Service"><p class="ServiceText poppins-text">Дизайн</p></div>
        </div>
        <div class="Line">
          <div class="Service"><p class="ServiceText poppins-text">Сайт под ключ</p></div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped> 
  .ServiceComponentHolst {
    width: 100%;
    min-height: 100vh;
    background-color: black;
    display: flex;
    justify-content: center;
  }

  .ServiceContainer {
    width: 90%;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
  }

  .ServiceContentContainer {
    width: 80%;
    height: 80%;

    display: flex;
    flex-direction: column;
    justify-content: center;
  }

  .Line {
    width: 100%;
    height: 15vh;

    display: flex;
    align-items: center;
    justify-content: center;

    border-top: 1px solid rgb(78, 78, 78);

  }

  .Service {
    width: 99%;
    height: 90%;
    display: flex;
    justify-content: left;
    align-items: center;

    transition: 0.3s;
  }

  .Service:hover {
    width: 98%;
    .ServiceText {
      text-shadow: 0px 0px 14px rgba(255, 255, 255, 0.58);
      color: rgb(208, 212, 220);
    }
  }

  .ServiceText {
    color: rgba(208, 212, 220, 0.655);  
    font-size: 48px;   
    line-height: 72px;
    transition: 0.3s;
  }

  /* шрифты */

  .oswald-text {
    font-family: 'Oswald', sans-serif;
    font-weight: 500;
  }

  .poppins-text {
    font-family: 'Poppins', sans-serif;
    font-weight: 600; 
    font-style: normal;
  }


</style>