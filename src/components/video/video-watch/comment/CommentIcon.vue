<template>
  <div>
    <svg
      id="comment-button"
      class="button"
      width="30"
      height="30"
      viewBox="0 0 48 48"
      fill="none"
      xmlns="http://www.w3.org/2000/svg"
    >
      <g opacity="0.9">
        <circle cx="24" cy="24" r="24" fill="#F1F1F2" />
        <g filter="url(#filter0_d)">
          <path
            fill-rule="evenodd"
            clip-rule="evenodd"
            d="M24.7501 33.3695C24.6669 33.3708 24.5835 33.3714 24.5 33.3714C17.0442 33.3714 11 28.3634 11 22.1857C11 16.008 17.0442 11 24.5 11C31.9558 11 38 16.008 38 22.1857C38 23.7481 37.6134 25.2356 36.9152 26.5863C35.9784 28.4958 34.5325 30.1999 32.9641 31.67C30.5745 33.91 27.8095 35.6008 25.8406 36.5538C25.3198 36.8059 24.7501 36.4112 24.7501 35.8615V33.3695ZM19.8714 22.7643C19.8714 23.8294 19.008 24.6929 17.9429 24.6929C16.8777 24.6929 16.0143 23.8294 16.0143 22.7643C16.0143 21.6992 16.8777 20.8357 17.9429 20.8357C19.008 20.8357 19.8714 21.6992 19.8714 22.7643ZM24.5 24.6929C25.5651 24.6929 26.4286 23.8294 26.4286 22.7643C26.4286 21.6992 25.5651 20.8357 24.5 20.8357C23.4349 20.8357 22.5714 21.6992 22.5714 22.7643C22.5714 23.8294 23.4349 24.6929 24.5 24.6929ZM32.9857 22.7643C32.9857 23.8294 32.1223 24.6929 31.0571 24.6929C29.992 24.6929 29.1286 23.8294 29.1286 22.7643C29.1286 21.6992 29.992 20.8357 31.0571 20.8357C32.1223 20.8357 32.9857 21.6992 32.9857 22.7643Z"
            fill="black"
          />
          <path
            fill-rule="evenodd"
            clip-rule="evenodd"
            d="M24.5 33.3714C31.9558 33.3714 38 28.3634 38 22.1857C38 16.008 31.9558 11 24.5 11C17.0442 11 11 16.008 11 22.1857C11 28.3634 17.0442 33.3714 24.5 33.3714ZM17.9429 24.6929C19.008 24.6929 19.8714 23.8294 19.8714 22.7643C19.8714 21.6992 19.008 20.8357 17.9429 20.8357C16.8777 20.8357 16.0143 21.6992 16.0143 22.7643C16.0143 23.8294 16.8777 24.6929 17.9429 24.6929ZM26.4286 22.7643C26.4286 23.8294 25.5651 24.6929 24.5 24.6929C23.4349 24.6929 22.5714 23.8294 22.5714 22.7643C22.5714 21.6992 23.4349 20.8357 24.5 20.8357C25.5651 20.8357 26.4286 21.6992 26.4286 22.7643ZM31.0571 24.6929C32.1223 24.6929 32.9857 23.8294 32.9857 22.7643C32.9857 21.6992 32.1223 20.8357 31.0571 20.8357C29.992 20.8357 29.1286 21.6992 29.1286 22.7643C29.1286 23.8294 29.992 24.6929 31.0571 24.6929Z"
            fill="black"
          />
        </g>
      </g>
      <defs>
        <filter
          id="filter0_d"
          x="7"
          y="7.33"
          width="35"
          height="33.6316"
          filterUnits="userSpaceOnUse"
          color-interpolation-filters="sRGB"
        >
          <feFlood flood-opacity="0" result="BackgroundImageFix" />
          <feColorMatrix
            in="SourceAlpha"
            type="matrix"
            values="0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 127 0"
          />
          <feOffset dy="0.33" />
          <feGaussianBlur stdDeviation="2" />
          <feColorMatrix
            type="matrix"
            values="0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0.1 0"
          />
          <feBlend
            mode="normal"
            in2="BackgroundImageFix"
            result="effect1_dropShadow"
          />
          <feBlend
            mode="normal"
            in="SourceGraphic"
            in2="effect1_dropShadow"
            result="shape"
          />
        </filter>
      </defs>
    </svg>
    <span class="numbers">{{ formatSuffixNumber(totalComments) }}</span>
  </div>
</template>
<script>
import { formatSuffixNumber } from "src/utils/number";
export default {
  props: ["totalComments"],
  methods: {
    formatSuffixNumber,
  },
};
</script>