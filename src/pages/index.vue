<template>
  <div class="d-flex flex-column justify-center align-center h-100">
    <VImg
      :src="pana"
      :height="500"
      width="500"
    />
    <h1>Boas vindas ao portal BankPyx</h1>
    <h6>Nosso sistema é responsivo, mas para obter uma melhor experiência aconselhamos acessar em um desktop</h6>
  </div>
</template>

<script setup>
import pana from '@images/svg/pana.svg'
</script>
