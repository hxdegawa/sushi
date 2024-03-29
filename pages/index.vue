<template lang="pug">
  .top
    FadeShutter(:timeout="5000")
    AnimatedLogo(:timeout="5000")
    
    .top__hero
      .inner-margin
        .top__hero__text
          .top__hero__text__title
            span お寿司なみんなと
            span ゲームを楽しむ。

          .top__hero__text__desc
            p 楽しむことを目的に、ゲーム内で友達を作って遊びましょう。
            p ときどきイベントを開催したりもします！

    .top__teams
      .top__teams__columns
        .top__teams__column(v-for="(sushi, name) in sushiData" :key="name" :class="sushi.en")
          .top__teams__text
            span.top__teams__text__title {{ `${name}チーム` }}
            span.top__teams__text__count {{ teamMembers[`${name}チーム`] }}
            span.top__teams__text__online-count {{ onlineCount[`${name}チーム`] }}

    .top__games
      .top__games__columns
        .top__games__column
          img(src="~/assets/images/games/league_of_legends.png" alt="game symbol").top__games__logo
          video(src="https://lolstatic-a.akamaihd.net/frontpage/apps/prod/harbinger-l10-website/ja-jp/production/ja-jp/static/hero-0632cbf2872c5cc0dffa93d2ae8a29e8.webm" muted loop autoplay).top__games__video
        .top__games__column
          img(src="~/assets/images/games/minecraft.png" alt="game symbol").top__games__logo
          video(src="~/assets/videos/minecraft.webm" muted loop autoplay).top__games__video
        .top__games__column
          img(src="~/assets/images/games/ark.png" alt="game symbol").top__games__logo
          video(src="~/assets/videos/ark.webm" muted loop autoplay).top__games__video
        .top__games__column
          img(src="~/assets/images/games/factorio.png" alt="game symbol").top__games__logo
          video(src="~/assets/videos/factorio.webm" muted loop autoplay).top__games__video
        
</template>

<script>
import FadeShutter from '~/components/FadeShutter'
import AnimatedLogo from '~/components/AnimatedLogo'
import sushiJSON from '~/lib/sushiTeams.json'

export default {
  components: {
    FadeShutter,
    AnimatedLogo
  },
  async asyncData({ $axios }) {
    const { teamData, onlineStatus } = await $axios.$get(`/api/teams`)

    return {
      teamInfo: teamData,
      onlineInfo: onlineStatus
    }
  },
  data() {
    return {
      sushiData: sushiJSON
    }
  },
  computed: {
    teamMembers() {
      const result = {}
      Object.keys(this.teamInfo).forEach((key) => {
        result[key] = this.teamInfo[key].length
      })
      return result
    },
    onlineCount() {
      const result = {}
      Object.keys(this.onlineInfo).forEach((key) => {
        result[key] = this.onlineInfo[key].length
      })
      return result
    }
  },
  mounted() {
    Object.keys(this.teamInfo).forEach((member) => {
      console.log(this.teamInfo[member])
    })
  }
}
</script>

<style lang="scss">
.top {
  &__hero {
    position: relative;
    display: block;
    height: 70vh;
    height: calc(var(--vh, 1vh) * 70);
    background-color: $color-yellow;

    &__text {
      position: absolute;
      left: 0;
      bottom: 40px;

      @media screen and (max-width: $pc + 20px) {
        left: 20px;
      }

      &__title {
        span {
          font-size: 50px;
          font-weight: bold;

          &:not(:first-of-type) {
            &::before {
              content: '\A';
              white-space: pre;
            }
          }
        }
      }

      &__desc {
        p {
          line-height: 30px;
          margin: 0;

          &:first-of-type {
            margin-top: 50px;
          }
        }
      }

      @media screen and (max-width: $tablet) {
        left: 20px;
        width: calc(100% - 40px);

        &__title {
          span {
            font-size: 40px;
          }
        }
      }

      @media screen and (max-width: $mobile) {
        left: 20px;
        width: calc(100% - 40px);

        &__title {
          span {
            font-size: 30px;
          }
        }
      }
    }

    .inner-margin {
      height: inherit;

      &:before {
        content: '';
        position: absolute;
        right: 0;
        top: 0;
        width: 120px;
        height: 100%;
        background-color: $color-lightblack;

        @media screen and (max-width: $mobile) {
          display: none;
        }
      }
    }
  }

  &__teams {
    &__columns {
      display: grid;
      grid-template-columns: repeat(5, 1fr);

      @media screen and (max-width: $tablet) {
        grid-template-columns: repeat(1, 1fr);
      }
    }

    &__column {
      position: relative;
      display: flex;
      justify-content: center;
      align-items: center;
      height: 70vh;
      height: calc(var(--vh, 1vh) * 30);
      padding-top: 50px;

      &:hover {
        .top__teams__text {
          &__online-count {
            opacity: 1;
            max-height: 20px;
          }
        }
      }

      &:before {
        position: absolute;
        content: '';
        top: 0;
        left: 0;
        height: 50px;
        width: 100%;
        display: block;
      }

      &.ikura {
        &:before {
          background-color: $color-ikura;
        }
      }

      &.uni {
        &:before {
          background-color: $color-uni;
        }
      }

      &.mayocorn {
        &:before {
          background-color: $color-mayocorn;
        }
      }

      &.shirasu {
        &:before {
          background-color: $color-shirasu;
        }
      }

      &.negitoro {
        &:before {
          background-color: $color-negitoro;
        }
      }
    }

    &__text {
      &__title {
        display: block;
        margin: 0 auto;
        font-size: 18px;
        font-weight: 700;
        text-align: center;
        letter-spacing: 2px;
      }

      &__count {
        display: block;
        font-size: 60px;
        font-weight: 700;
        text-align: center;

        &::after {
          content: '貫';
          font-size: 18px;
        }
      }

      &__online-count {
        display: block;
        font-size: 9px;
        line-height: 20px;
        font-weight: 700;
        text-align: center;
        max-height: 0px;
        opacity: 0;
        pointer-events: none;
        transition: 0.2s ease;

        &::before {
          content: '';
          display: inline-block;
          vertical-align: top;
          height: 8px;
          width: 8px;
          margin: 6px;
          border-radius: 7px;
          background-color: $color-green;
        }

        &::after {
          content: '貫がオンライン';
          font-weight: 400;
        }
      }
    }
  }

  &__games {
    &__columns {
      display: grid;
      grid-template-columns: repeat(2, 1fr);
      min-height: 100vh;
      min-height: calc(var(--vh, 1vh) * 100);

      @media screen and (max-width: $tablet) {
        grid-template-columns: repeat(1, 1fr);
      }
    }

    &__column {
      position: relative;
      display: flex;
      justify-content: center;
      align-items: center;
      min-height: 50vh;
      min-height: calc(var(--vh, 1vh) * 50);
      overflow: hidden;
      filter: grayscale(1);
      transition: filter 0.4s ease;

      &:hover {
        filter: grayscale(0);
      }

      @media screen and (max-width: $tablet) {
        padding: 20px;
      }
    }

    &__logo {
      z-index: 2;
      object-fit: cover;
      max-width: 450px;

      @media screen and (max-width: $mobile) {
        max-width: 100%;
      }
    }

    &__video {
      position: absolute;
      left: 0;
      top: 0;
      width: 100%;
      height: 100%;
      object-fit: cover;
      z-index: 1;
    }
  }
}
</style>
