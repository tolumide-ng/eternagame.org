<template>
  <div>
    <AspectRatioCard :aspectRatio="aspectRatio" :id="`popover-target-${nid}`" class="card">
      <template #header>
        <div class="puzzle-card-title" v-if="title">
          <b>{{ title }}</b>
        </div>
      </template>
      <img :src="imageURL" style="width: 80%; margin: auto;" class="scalable" />
      <img src="@/assets/noun_lock.svg" v-if="locked" class="inner" />
      <template #footer>
        <b-row class="mb-2">
          <b-col cols="4" class="left-col" v-if="reward">
            <slot name="left-icon">
              <img src="@/assets/dollar.svg" alt="reward slots" class="icon" />
            </slot>
            {{ reward }}
          </b-col>
          <b-col cols="4" class="text-center" v-if="states">
            <StateCounter :value="states" />
          </b-col>
          <b-col cols="4" class="right-col" v-if="numCleared">
            <slot name="right-icon">
              <img src="@/assets/people.svg" alt="submissions" class="icon" />
            </slot>
            {{ numCleared }}
          </b-col>
        </b-row>
        <div style="width: 100%;" class="d-flex justify-content-between" v-if="$slots.buttons">
          <slot name="buttons" />
        </div>
      </template>
    </AspectRatioCard>
    <b-popover
      v-if="!title"
      :target="`popover-target-${nid}`"
      placement="bottom"
      style="width: 214.2px; height: 110.2px;"
    >
      <p style="font-size: 1.3333333333333333rem;">RNA Slicer</p>
      <b-button
        type="button"
        class="btn-success"
        style="width: 152px; height: 40px; margin: 8px 0px;"
        >Play</b-button
      >
    </b-popover>
  </div>
</template>
<script lang="ts">
  import { Component, Prop, Vue } from 'vue-property-decorator';
  import Utils from '@/utils/utils';
  import AspectRatioCard from './AspectRatioCard.vue';
  import StateCounter from './StateCounter.vue';

  @Component({
    components: {
      StateCounter,
      AspectRatioCard,
    },
  })
  export default class PuzzleCard extends Vue {
    @Prop() private title!: string;

    @Prop() private nid!: string;

    @Prop() private reward!: number;

    @Prop({ default: 0 }) private states!: number;

    @Prop() private image!: string;

    @Prop({ default: 1 }) private aspectRatio!: number;

    @Prop({ default: false }) private locked!: boolean;

    get numCleared() {
      return this.$attrs['num-cleared'];
    }

    get imageURL() {
      return this.image
        ? `${process.env.VUE_APP_API_BASE_URL}${this.image}`
        : Utils.getPuzzleMiddleThumbnail(this.nid);
    }
  }
</script>

<style lang="scss" scoped>
  .btn {
    display: inline-block;
    width: 48%;
    margin-bottom: 0px;
  }

  ::v-deep .card-body {
    padding: 0.75rem !important;
  }

  .puzzle-card-title {
    height: 50px;
    text-align: center;
  }

  .middle-num {
    position: absolute;
    top: Calc(50%);
    left: 50%;
    transform: translate(-50%, -50%);
    color: #21508c;
    font-size: 8px;
    font-weight: 900;
  }

  .icon {
    width: 19px;
    margin-right: 5px;
  }

  .left-col,
  .right-col {
    font-size: 11px;
    display: flex;
    align-items: center;
    font-weight: bold;

    & > .icon {
      width: 12.57px;
    }
  }

  .right-col {
    justify-content: flex-end;
  }

  .inner {
    left: 50%;
    transform: translate(-50%, -50%);
    top: 50%;
    position: absolute;
    z-index: 1;
  }

  .card:hover {
    border: 1px solid gold;
  }
</style>
