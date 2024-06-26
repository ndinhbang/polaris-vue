<template lang="pug">
router-link(
  v-if="to",
  :to="to",
  :class="className",
  :aria-label="accessibilityLabel",
  :data-primary-link="dataPrimaryLink",
)
  slot

UnstyledLink(
  v-else-if="url",
  :class="className",
  :url="url",
  :external="external",
  :target="target",
  :aria-label="accessibilityLabel",
  :data-primary-link="dataPrimaryLink ?? undefined",
  @click="emits('click')",
)
  slot

button(
  v-else,
  type="button",
  :class="className",
  :aria-label="accessibilityLabel",
  :data-primary-link="dataPrimaryLink ?? undefined",
  @click="emits('click')",
)
  slot
</template>

<script setup lang="ts">
import {
  inject,
  computed,
} from 'vue';
import { UnstyledLink } from '@/components';
import { classNames } from '@/utilities/css';
import type {
  Target,
  VueNode,
} from '@/utilities/types';
import type { RouteLocationRaw } from './types';
import styles from '@polaris/components/Link/Link.module.scss';

export type LinkProps = {
  /** ID for the link */
  id?: string;
  /** The url to link to */
  url?: string;
  /**
   * Router link 'to' option, if this prop is provided router-link component will be render
   * <a href="https://router.vuejs.org/api/interfaces/RouterLinkProps.html#to" target="blank"> details </a>
   */
  to?: RouteLocationRaw
  /** Makes the link open in a new tab
   * @deprecated use `target` set to `_blank` instead
   */
  external?: boolean;
  /** Where to display the url */
  target?: Target;
  /** Makes the link color the same as the current text color and adds an underline */
  monochrome?: boolean;
  /** Removes text decoration underline to the link */
  removeUnderline?: boolean;
  /** Descriptive text to be read to screenreaders */
  accessibilityLabel?: string;
  /** Indicates whether or not the link is the primary navigation link when rendered inside of an `IndexTable.Row` */
  dataPrimaryLink?: boolean;
}

const bannerContext = inject('banner-context', false);

const props = defineProps<LinkProps>();

defineSlots<{
  /** Content to display inside the link */
  default: (_: VueNode) => null;
}>();

const emits = defineEmits<{
  /** Callback when a link is clicked */
  click: [];
}>();

const shouldBeMonochrome = computed(() => Boolean(props.monochrome || bannerContext));

const className = computed(() => classNames(
  styles.Link,
  shouldBeMonochrome.value && styles.monochrome,
  props.removeUnderline && styles.removeUnderline,
));
</script>
