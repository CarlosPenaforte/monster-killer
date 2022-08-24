<template>
  <div class="game">
    <div class="game-screen">
      <div class="players">
        <div class="player" v-for="(player, index) in players" :key="index">
          <h2>{{ player.nome }}</h2>
          <img :src="player.img" style="height: 150px" />
          <div class="barra-de-vida">
            <div
              class="vida"
              :style="{
                width: player.vida + '%',
                backgroundColor: player.fundo,
              }"
            >
              {{ `${player.vida}%` }}
            </div>
          </div>
        </div>
      </div>
      <div class="actions">
        <div
          class="action"
          v-for="(action, index) in actions"
          :key="index"
          @click="action[1]"
          :class="{ disable }"
        >
          {{ action[0] }}
        </div>
        <div class="giveup" @click="reiniciar">{{ restart }}</div>
      </div>
    </div>
    <ActionsLog :logs="log" />
  </div>
</template>

<script>
  import ActionsLog from "./ActionsLog.vue";
  import image1 from "../assets/img/cavaleiro.webp";
  import image2 from "../assets/img/dragao.webp";

  export default {
    name: "GameScreen",
    components: {
      ActionsLog,
    },
    data() {
      return {
        players: [
          {
            nome: "Cavaleiro",
            img: image1,
            vida: 100,
            attMin: 5,
            attMax: 10,
            attEspMin: 8,
            attEspMax: 13,
            fundo: "rgb(30, 255, 30)",
          },
          {
            nome: "Dragão",
            img: image2,
            vida: 100,
            attMin: 7,
            attMax: 12,
            fundo: "rgb(30, 255, 30)",
          },
        ],
        log: [],
        damage: 0,
        cura: 0,
        actions: [
          ["Ataque", this.ataque],
          ["Ataque Especial", this.ataqueEspecial],
          ["Cura", this.curar],
        ],
        disable: false,
        restart: "Desistir",
      };
    },
    methods: {
      ataque() {
        if (this.players[0].vida !== 0 && this.players[1].vida !== 0) {
          this.damage = Math.floor(
            Math.random() * (this.players[1].attMax - this.players[1].attMin) +
              this.players[1].attMin
          );
          this.players[0].vida -=
            this.damage >= this.players[0].vida
              ? this.players[0].vida
              : this.damage;
          if (this.players[0].vida <= 50) {
            this.players[0].fundo = "rgb(255, 30, 30)";
          }
          this.log.unshift(
            `${this.players[1].nome} atacou e causou ${this.damage} de dano`
          );
          this.damage = Math.floor(
            Math.random() * (this.players[0].attMax - this.players[0].attMin) +
              this.players[0].attMin
          );
          this.players[1].vida -=
            this.damage >= this.players[1].vida
              ? this.players[1].vida
              : this.damage;
          if (this.players[1].vida <= 50) {
            this.players[1].fundo = "rgb(255, 30, 30)";
          }
          this.log.unshift(
            `${this.players[0].nome} atacou e causou ${this.damage} de dano`
          );

          if (this.players[0].vida === 0) {
            this.disable = true;
            this.restart = "Reiniciar";
            this.log.unshift("Você morreu!");
          } else if (this.players[1].vida === 0) {
            this.disable = true;
            this.restart = "Reiniciar";
            this.log.unshift("Você venceu!");
          }
        }
      },
      ataqueEspecial() {
        if (this.players[0].vida !== 0 && this.players[1].vida !== 0) {
          this.damage = Math.floor(
            Math.random() * (this.players[1].attMax - this.players[1].attMin) +
              this.players[1].attMin
          );
          this.players[0].vida -=
            this.damage >= this.players[0].vida
              ? this.players[0].vida
              : this.damage;
          if (this.players[0].vida <= 50) {
            this.players[0].fundo = "rgb(255, 30, 30)";
          }
          this.log.unshift(
            `${this.players[1].nome} atacou e causou ${this.damage} de dano`
          );
          this.damage = Math.floor(
            Math.random() *
              (this.players[0].attEspMax - this.players[0].attEspMin) +
              this.players[0].attEspMin
          );
          this.players[1].vida -=
            this.damage >= this.players[1].vida
              ? this.players[1].vida
              : this.damage;
          if (this.players[1].vida <= 50) {
            this.players[1].fundo = "rgb(255, 30, 30)";
          }
          this.log.unshift(
            `${this.players[0].nome} atacou e causou ${this.damage} de dano`
          );

          if (this.players[0].vida === 0) {
            this.disable = true;
            this.restart = "Reiniciar";
            this.log.unshift("Você morreu!");
          } else if (this.players[1].vida === 0) {
            this.disable = true;
            this.restart = "Reiniciar";
            this.log.unshift("Você venceu!");
          }
        }
      },
      curar() {
        if (this.players[0].vida !== 0 && this.players[1].vida !== 0) {
          this.cura = Math.floor(Math.random() * 5 + 7);
          this.log.unshift(
            `${this.players[0].nome} curou-se em ${this.cura} de vida`
          );
          this.damage = Math.floor(
            Math.random() * (this.players[1].attMax - this.players[1].attMin) +
              this.players[1].attMin
          );
          this.players[0].vida -=
            this.damage - this.cura >= this.players[0].vida
              ? this.players[0].vida
              : this.damage - this.cura;
          if (this.players[0].vida <= 50) {
            this.players[0].fundo = "rgb(255, 30, 30)";
          } else {
            this.players[0].fundo = "rgb(30, 255, 30)";
          }
          this.log.unshift(
            `${this.players[1].nome} atacou e causou ${this.damage} de dano`
          );

          if (this.players[0].vida === 0) {
            this.disable = true;
            this.restart = "Reiniciar";
            this.log.unshift("Você morreu!");
          } else if (this.players[1].vida === 0) {
            this.disable = true;
            this.restart = "Reiniciar";
            this.log.unshift("Você venceu!");
          }
        }
      },
      reiniciar() {
        this.players[0].vida = 100;
        this.players[1].vida = 100;
        this.players[0].fundo = "rgb(30, 255, 30)";
        this.players[1].fundo = "rgb(30, 255, 30)";
        this.log = [];
        this.disable = false;
        this.restart = "Desistir";
      },
    },
  };
</script>

<style scoped>
  .game {
    display: flex;
    flex-direction: column;
    align-items: center;
    margin: 0;
    padding: 0;
    width: 70%;
  }

  .game-screen {
    display: flex;
    flex-direction: column;
    align-items: center;
    width: 100%;
    border: 2px solid rgb(10, 30, 30);
    background-color: rgb(255, 245, 245);
    box-sizing: border-box;
    padding: 2rem;
  }

  .players {
    display: flex;
    justify-content: space-around;
    width: 100%;
    margin: 0;
    padding: 0;
    margin-bottom: 3rem;
  }

  .player {
    padding: 0;
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  .player h2 {
    margin: 0;
    padding: 0;
    color: rgb(10, 30, 30);
  }

  .player img {
    margin: 0.5rem;
    border-radius: 10px;
  }

  .barra-de-vida {
    width: 12rem;
    border: 1px solid rgb(10, 30, 30);
  }

  .vida {
    text-align: end;
  }

  .actions {
    display: flex;
    width: 100%;
    justify-content: space-evenly;
  }

  .action {
    font-family: "Source Sans Pro", sans-serif;
    width: 8rem;
    padding: 0.5rem;
    background-color: rgb(50, 150, 150);
    box-shadow: 3px 3px 0 rgb(10, 30, 30);
    color: rgb(255, 245, 245);
    font-weight: 500;
    display: flex;
    justify-content: center;
    align-items: center;
    cursor: pointer;
    user-select: none;
  }

  .action:hover {
    background-color: rgb(60, 180, 180);
  }

  .action:active {
    background-color: rgb(50, 150, 150);
  }

  .giveup {
    font-family: "Source Sans Pro", sans-serif;
    width: 8rem;
    padding: 0.5rem;
    background-color: rgb(200, 50, 50);
    box-shadow: 3px 3px 0 rgb(10, 30, 30);
    color: rgb(255, 245, 245);
    font-weight: 500;
    display: flex;
    justify-content: center;
    align-items: center;
    cursor: pointer;
    user-select: none;
  }

  .giveup:hover {
    background-color: rgb(250, 100, 100);
  }

  .giveup:active {
    background-color: rgb(200, 50, 50);
  }

  .disable {
    background-color: rgb(150, 150, 150);
  }

  .disable:hover {
    background-color: rgb(150, 150, 150);
  }

  .disable:active {
    background-color: rgb(150, 150, 150);
  }
</style>
