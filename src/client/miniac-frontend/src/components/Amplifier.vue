<script setup lang="ts">
import { inject, computed } from "vue";
import type {IMiniac} from "../plugins/Miniac/IMiniac";
import IconPower from "./icons/IconPower.vue";
import IconNotes from "./icons/IconNotes.vue";

const miniac = inject<IMiniac>("miniac");
if (miniac === undefined)
{
  throw new Error ("Amplifier.setup: No miniac plugin found.");
}

const amp_SelectedInput = computed(() =>
{
  return miniac.amp_IsOn.value ? miniac.amp_SelectedInput.value : "";
});
</script>

<template>
  <div class="amplifier">
    <button :class="{'power-on': miniac.amp_IsOn.value}"
            style="grid-area: power;"
            @click="miniac.amp_RequestPowerToggle()">
      <IconPower />
    </button>
    <div style="grid-area: input;">{{ amp_SelectedInput }}</div>
    <div style="grid-area: x;"></div>
    <div style="grid-area: y;"></div>
    <button :class="{'power-on': amp_SelectedInput === 'MINIAC'}"
            style="grid-area: selector;"
            @click="miniac.amp_SetSelectedInput('MINIAC')">
      <IconNotes />
    </button>
  </div>
</template>

<style scoped>
.amplifier
{
  font-size: 0.6rem;
  display: grid;
  grid-template-columns: 4rem 4rem auto;
  grid-template-areas: 
    "power selector input"
    "power selector x    "
    "power selector y    "
  ;
  gap: 0.5rem;
}

.power-on svg
{
    color: var(--color-power);
    filter: drop-shadow(0 0 0.2rem currentColor);
}
</style>
