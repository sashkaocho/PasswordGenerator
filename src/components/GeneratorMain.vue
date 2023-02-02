<script setup lang="ts">
import { ref, computed } from 'vue';

const randPasswordVisibility = ref<boolean>(true);
const copyVisibility = ref<boolean>(false);

const characterLength = ref<number>(0);
const checkedOptions = ref<number>(0);
const allLetters = ref<string[]>([]);

const generatedPassword = ref<string>('');

const options = ref<any[]>([
  {
    value: false,
    letters: [
      'A',
      'B',
      'C',
      'D',
      'E',
      'F',
      'G',
      'H',
      'I',
      'J',
      'K',
      'L',
      'M',
      'N',
      'O',
      'P',
      'Q',
      'R',
      'S',
      'T',
      'U',
      'V',
      'W',
      'X',
      'Y',
      'Z',
    ],
  },
  {
    value: false,
    letters: [
      'a',
      'b',
      'c',
      'd',
      'e',
      'f',
      'g',
      'h',
      'i',
      'j',
      'k',
      'l',
      'm',
      'n',
      'o',
      'p',
      'q',
      'r',
      's',
      't',
      'u',
      'v',
      'w',
      'x',
      'y',
      'z',
    ],
  },
  { value: false, letters: ['0', '1', '2', '3', '4', '5', '6', '7', '8', '9'] },
  {
    value: false,
    letters: [
      '*',
      '+',
      '-',
      '/',
      '?',
      '!',
      ')',
      '(',
      '#',
      '$',
      '%',
      '|',
      '^',
      '@',
      '.',
      ',',
      '&',
      '=',
      '{',
      '}',
      '~',
      ':',
      ';',
    ],
  },
]);

const stengthLevel = computed(() => {
  switch (checkedOptions.value) {
    case 1:
      return 'too weak!';
    case 2:
      return 'weak';
    case 3:
      return 'medium';
    case 4:
      return 'strong';
  }
});

const generatePassword = (): void => {
  const chk = options.value.filter((item) => item.value === true);

  for (let i = 0; i < chk.length; i++) {
    if (chk.length === 1) {
      allLetters.value = chk[i]?.letters;
    }
    if (chk.length === 2) {
      allLetters.value = chk[i]?.letters.concat(chk[i - 1]?.letters);
    }
    if (chk.length === 3) {
      allLetters.value = chk[i]?.letters.concat(
        chk[i - 1]?.letters,
        chk[i - 2]?.letters
      );
    }
    if (chk.length === 4) {
      allLetters.value = chk[i]?.letters.concat(
        chk[i - 1]?.letters,
        chk[i - 2]?.letters,
        chk[i - 3]?.letters
      );
    }
  }

  copyVisibility.value = false;
  randPasswordVisibility.value = false;
  generatedPassword.value = generator(characterLength.value, allLetters.value);
};

const generator = (len: number, arr: string[]) => {
  let str = '';
  for (let i = 0; i < len; i++) {
    str = str + random(arr);
  }
  if (arr.length === 0) {
    return '';
  }
  return str;
};

const random = (arr: string[]) => {
  if (arr === undefined) {
    return '';
  }
  return arr[Math.floor(Math.random() * arr.length)];
};

const check = (): void => {
  const checked = options.value.filter((item) => item.value === true);
  checkedOptions.value = checked.length;
};

const setValue = (event: any) => (characterLength.value = event.target.value);

const copyText = (): void => {
  copyVisibility.value = true;
  try {
    navigator.clipboard.writeText(generatedPassword.value);
  } catch (err) {
    throw err;
  }
};
</script>

<template>
  <main class="generator">
    <h2>Password Generator</h2>
    <section class="generator-password">
      <h1 v-if="randPasswordVisibility" class="password">P4$5W0rD!</h1>
      <h1 v-else>{{ generatedPassword }}</h1>
      <div class="password-copy">
        <h3 v-if="copyVisibility">copied</h3>
        <svg
          @click="copyText"
          width="21"
          height="24"
          xmlns="http://www.w3.org/2000/svg"
        >
          <path
            d="M20.341 3.091 17.909.659A2.25 2.25 0 0 0 16.319 0H8.25A2.25 2.25 0 0 0 6 2.25V4.5H2.25A2.25 2.25 0 0 0 0 6.75v15A2.25 2.25 0 0 0 2.25 24h10.5A2.25 2.25 0 0 0 15 21.75V19.5h3.75A2.25 2.25 0 0 0 21 17.25V4.682a2.25 2.25 0 0 0-.659-1.591ZM12.469 21.75H2.53a.281.281 0 0 1-.281-.281V7.03a.281.281 0 0 1 .281-.281H6v10.5a2.25 2.25 0 0 0 2.25 2.25h4.5v1.969a.282.282 0 0 1-.281.281Zm6-4.5H8.53a.281.281 0 0 1-.281-.281V2.53a.281.281 0 0 1 .281-.281H13.5v4.125c0 .621.504 1.125 1.125 1.125h4.125v9.469a.282.282 0 0 1-.281.281Zm.281-12h-3v-3h.451c.075 0 .147.03.2.082L18.667 4.6a.283.283 0 0 1 .082.199v.451Z"
            fill="currentColor"
          />
        </svg>
      </div>
    </section>
    <section class="generator-options">
      <article class="character">
        <div class="character-container">
          <h3>Character Length</h3>
          <h1>{{ characterLength }}</h1>
        </div>
        <input
          class="character-range"
          type="range"
          value="0"
          min="0"
          max="20"
          @mousemove="setValue"
        />
      </article>
      <article class="options" @change="check">
        <div class="option">
          <input
            type="checkbox"
            class="option-check"
            v-model="options[0].value"
          />
          <h3>Include Uppercase Letters</h3>
        </div>
        <div class="option">
          <input
            type="checkbox"
            class="option-check"
            v-model="options[1].value"
          />
          <h3>Include Lowercase Letters</h3>
        </div>
        <div class="option">
          <input
            type="checkbox"
            class="option-check"
            v-model="options[2].value"
          />
          <h3>Include Numbers</h3>
        </div>
        <div class="option">
          <input
            type="checkbox"
            class="option-check"
            v-model="options[3].value"
          />
          <h3>Include Symbols</h3>
        </div>
      </article>
      <article class="password-strength">
        <h3>strength</h3>
        <div class="strength-container">
          <h2>{{ stengthLevel }}</h2>
          <div class="strength-phases">
            <div
              class="strength-phase"
              :class="{
                tooweak: checkedOptions === 1,
                weak: checkedOptions === 2,
                medium: checkedOptions === 3,
                strong: checkedOptions === 4,
              }"
            ></div>
            <div
              class="strength-phase"
              :class="{
                weak: checkedOptions === 2,
                medium: checkedOptions === 3,
                strong: checkedOptions === 4,
              }"
            ></div>
            <div
              class="strength-phase"
              :class="{
                medium: checkedOptions === 3,
                strong: checkedOptions === 4,
              }"
            ></div>
            <div
              class="strength-phase"
              :class="{ strong: checkedOptions === 4 }"
            ></div>
          </div>
        </div>
      </article>
      <button class="generate-btn" @click="generatePassword">
        <h3>generate</h3>
        <svg width="12" height="12" xmlns="http://www.w3.org/2000/svg">
          <path
            fill="currentColor"
            d="m5.106 12 6-6-6-6-1.265 1.265 3.841 3.84H.001v1.79h7.681l-3.841 3.84z"
          />
        </svg>
      </button>
    </section>
  </main>
</template>

<style scoped lang="scss">
.generator {
  width: 33.75rem;
  height: fit-content;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 1.5rem;

  h2 {
    color: $grey;
  }
}
.generator-password {
  height: 5rem;
  width: 100%;
  background: $dark-grey;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0 2rem;

  h1 {
    color: $almost-white;
  }
  h1.password {
    opacity: 0.25;
  }

  .password-copy {
    display: flex;
    align-items: center;
    gap: 1rem;

    h3 {
      text-transform: uppercase;
      color: $neon-green;
      cursor: pointer;
    }

    svg {
      cursor: pointer;
      color: $neon-green;
      &:active {
        color: $almost-white;
      }
    }
  }
}
.generator-options {
  width: 100%;
  height: 33rem;
  background: $dark-grey;
  padding: 1.5rem 2rem;
  display: flex;
  flex-direction: column;
  gap: 2rem;

  .character {
    width: 100%;
    height: 5rem;
    display: flex;
    flex-direction: column;
    justify-content: space-between;

    .character-container {
      display: flex;
      justify-content: space-between;
      align-items: center;
    }

    h3 {
      color: $almost-white;
    }

    h1 {
      color: $neon-green;
    }
  }

  .character-range {
    width: 100%;
    height: 0.5rem;
    appearance: none;
    -webkit-appearance: none;
    background: $very-dark-grey;
    &::-webkit-slider-thumb {
      -webkit-appearance: none;
      appearance: none;
      width: 28px;
      height: 28px;
      border-radius: 50%;
      cursor: grab;
      background: $almost-white;
      transition: 0.15s ease-in-out;
      &:hover {
        box-shadow: inset 0 0 0 2px $neon-green;
        background: $very-dark-grey;
      }
      &:active {
        cursor: grabbing;
      }
    }
    &::-moz-range-progress {
      background: $neon-green;
    }
  }

  .options {
    width: 100%;
    height: 9.5rem;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    margin-top: 0.5rem;

    .option {
      display: flex;
      align-items: center;
      gap: 1.5rem;

      .option-check {
        appearance: 0;
        -webkit-appearance: none;
        height: 20px;
        width: 20px;
        border: 2px solid $almost-white;
        cursor: pointer;
        display: flex;
        justify-content: center;
        align-items: flex-end;
        transition: 0.15s ease-in-out;

        &:hover {
          border-color: $neon-green;
        }
        &::after {
          content: url('../assets/svg/icon-check.svg');
          display: none;
          height: 0.85rem;
        }
        &:checked {
          border-color: $neon-green;
          background: $neon-green;
        }
        &:checked::after {
          display: block;
        }
      }

      h3 {
        color: $almost-white;
      }
    }
  }

  .password-strength {
    width: 100%;
    height: 4.5rem;
    background: $very-dark-grey;
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 0 2rem;

    h3 {
      text-transform: uppercase;
      color: $grey;
    }
    .strength-container {
      display: flex;
      align-items: center;
      gap: 1rem;

      h2 {
        text-transform: uppercase;
        color: $almost-white;
      }
      .strength-phases {
        width: 3.969rem;
        height: 1.75rem;
        display: flex;
        justify-content: space-between;

        .strength-phase {
          width: 0.625rem;
          height: 100%;
          border: 2px solid $almost-white;
          transition: 0.15s ease-in-out;
        }
        .tooweak {
          border-color: $red;
          background: $red;
        }
        .weak {
          border-color: $orange;
          background: $orange;
        }
        .medium {
          border-color: $yellow;
          background: $yellow;
        }
        .strong {
          border-color: $neon-green;
          background: $neon-green;
        }
      }
    }
  }

  .generate-btn {
    width: 100%;
    height: 4.063rem;
    background: $neon-green;
    border: 2px solid $neon-green;
    display: flex;
    justify-content: center;
    align-items: center;
    gap: 1.5rem;
    transition: 0.2s ease-in-out;
    cursor: pointer;
    &:hover {
      background: none;
      h3 {
        color: $neon-green;
      }
      svg {
        color: $neon-green;
      }
    }

    h3 {
      text-transform: uppercase;
      color: $dark-grey;
    }
  }
}

@media (max-width: 35rem) {
  .generator {
    width: 22.438rem;
    .strength-container {
      h2 {
        width: 3rem;
      }
    }
  }
}
</style>
