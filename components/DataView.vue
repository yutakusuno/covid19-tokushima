<template>
  <v-card class="DataView">
    <div class="DataView-Inner">
      <div class="DataView-Header">
        <h3
          class="DataView-Title"
          :class="!!$slots.infoPanel ? 'with-infoPanel' : ''"
        >
          {{ title }}
        </h3>
        <slot name="infoPanel" />
        <slot name="button" />
      </div>
      <div class="DataView-CardText">
        <slot />
      </div>
      <div class="DataView-Footer">
        <div class="Footer-Left">
          <div>
            <a
              v-if="url"
              class="OpenDataLink"
              :href="url"
              target="_blank"
              rel="noopener"
            >
              {{ $t('オープンデータを入手') }}
              <v-icon
                class="ExternalLinkIcon"
                size="15"
                :aria-label="this.$t('別タブで開く')"
                role="img"
                :aria-hidden="false"
              >
                mdi-open-in-new
              </v-icon>
            </a>
          </div>
          <div>
            <a class="Permalink" :href="permalink()">
              <time :datetime="formattedDate">
                {{ $t('{date} 更新', { date }) }}
              </time>
            </a>
          </div>
        </div>

        <div v-if="this.$route.query.embed != 'true'" class="Footer-Right">
          <div v-if="displayShare" class="DataView-Share-Buttons py-2">
            <div class="Close-Button">
              <v-icon @click="closeShareMenu">
                mdi-close
              </v-icon>
            </div>

            <h4>{{ $t('埋め込み用コード') }}</h4>

            <div class="EmbedCode">
              <v-icon
                v-if="isCopyAvailable()"
                class="EmbedCode-Copy"
                @click="copyEmbedCode"
              >
                far fa-clipboard
              </v-icon>
              {{ graphEmbedValue }}
            </div>

            <div class="Buttons">
              <button @click="line">
                <img src="/line.png" class="icon-resize line" />
              </button>

              <button @click="twitter">
                <img src="/twitter.png" class="icon-resize twitter" />
              </button>

              <button @click="facebook">
                <img src="/facebook.png" class="icon-resize facebook" />
              </button>
            </div>
          </div>
          <div class="DataView-Share-Opener" @click="toggleShareMenu">
            <svg
              width="14"
              height="16"
              viewBox="0 0 14 16"
              fill="none"
              xmlns="http://www.w3.org/2000/svg"
            >
              <path
                fill-rule="evenodd"
                clip-rule="evenodd"
                d="M7.59999 3.5H9.5L7 0.5L4.5 3.5H6.39999V11H7.59999V3.5ZM8.5 5.75H11.5C11.9142 5.75 12.25 6.08579 12.25 6.5V13.5C12.25 13.9142 11.9142 14.25 11.5 14.25H2.5C2.08579 14.25 1.75 13.9142 1.75 13.5V6.5C1.75 6.08579 2.08579 5.75 2.5 5.75H5.5V4.5H2.5C1.39543 4.5 0.5 5.39543 0.5 6.5V13.5C0.5 14.6046 1.39543 15.5 2.5 15.5H11.5C12.6046 15.5 13.5 14.6046 13.5 13.5V6.5C13.5 5.39543 12.6046 4.5 11.5 4.5H8.5V5.75Z"
                fill="#808080"
              />
            </svg>
          </div>
        </div>
      </div>
    </div>

    <div v-if="showOverlay" class="overlay">
      <div class="overlay-text">
        {{ $t('埋め込みコードをコピーしました') }}
      </div>
    </div>
  </v-card>
</template>

<script lang="ts">
import Vue from 'vue'
import { convertDatetimeToISO8601Format } from '@/utils/formatDate'

export default Vue.extend({
  props: {
    title: {
      type: String,
      default: ''
    },
    titleId: {
      type: String,
      default: ''
    },
    date: {
      type: String,
      default: ''
    },
    url: {
      type: String,
      default: ''
    }
  },
  data() {
    return {
      openGraphEmbed: false,
      displayShare: false,
      showOverlay: false
    }
  },
  computed: {
    formattedDate(): string {
      return convertDatetimeToISO8601Format(this.date)
    },
    graphEmbedValue(): string {
      const graphEmbedValue =
        '<iframe width="560" height="315" src="' +
        this.permalink(true, true) +
        '" frameborder="0"></iframe>'
      return graphEmbedValue
    }
  },
  methods: {
    toggleShareMenu() {
      this.displayShare = !this.displayShare
    },
    closeShareMenu() {
      this.displayShare = false
    },
    isCopyAvailable() {
      return !!navigator.clipboard
    },
    copyEmbedCode() {
      const self = this
      navigator.clipboard.writeText(this.graphEmbedValue).then(() => {
        self.closeShareMenu()

        self.showOverlay = true
        setTimeout(() => {
          self.showOverlay = false
        }, 2000)
      })
    },
    permalink(host: boolean = false, embed: boolean = false) {
      let permalink = '/cards/' + this.titleId
      if (embed) {
        permalink = permalink + '?embed=true'
      }
      permalink = this.localePath(permalink)

      if (host) {
        permalink = location.protocol + '//' + location.host + permalink
      }
      return permalink
    },
    twitter() {
      const url =
        'https://twitter.com/intent/tweet?text=' +
        this.title +
        ' / ' +
        this.$t('徳島県') +
        this.$t('新型コロナウイルス感染症') +
        this.$t('対策サイト') +
        '&url=' +
        this.permalink(true) +
        '&' +
        'hashtags=StopCovid19JP'
      window.open(url)
    },
    facebook() {
      const url =
        'https://www.facebook.com/sharer.php?u=' + this.permalink(true)
      window.open(url)
    },
    line() {
      const url =
        'https://social-plugins.line.me/lineit/share?url=' +
        this.permalink(true)
      window.open(url)
    }
  }
})
</script>

<style lang="scss">
.DataView {
  @include card-container();
  height: 100%;
  &-Header {
    display: flex;
    align-items: flex-start;
    flex-flow: column;
    padding: 0 10px;
    @include largerThan($medium) {
      padding: 0 5px;
    }
    @include largerThan($large) {
      width: 100%;
      flex-flow: row;
      flex-wrap: wrap;
      padding: 0;
    }
  }
  &-DataInfo {
    &-summary {
      color: $gray-2;
      font-family: Hiragino Sans;
      font-style: normal;
      font-size: 30px;
      line-height: 30px;
      white-space: nowrap;
      &-unit {
        font-size: 0.6em;
        width: 100%;
      }
    }
    &-date {
      font-size: 12px;
      line-height: 12px;
      color: $gray-3;
      width: 100%;
      display: inline-block;
    }
  }
}
.DataView {
  &-Inner {
    display: flex;
    flex-flow: column;
    justify-content: space-between;
    padding: 22px;
    height: 100%;
  }
  &-Title {
    width: 100%;
    margin-bottom: 10px;
    font-size: 1.25rem;
    line-height: 1.5;
    font-weight: normal;
    color: $gray-2;
    @include largerThan($large) {
      width: 50%;
      margin-bottom: 0;
    }
  }
  &-CardText {
    margin: 16px 0;
  }
  &-CardTextForXS {
    margin-bottom: 46px;
    margin-top: 70px;
  }
  &-Embed {
    background-color: $gray-5;
  }
  &-Footer {
    @include font-size(12);
    padding: 0 !important;
    display: flex;
    justify-content: space-between;
    color: $gray-3 !important;
    text-align: right;
    background-color: $white !important;

    .Permalink {
      color: $gray-3 !important;
    }
    .OpenDataLink {
      text-decoration: none;
      .ExternalLinkIcon {
        vertical-align: text-bottom;
      }
    }

    .Footer-Left {
      text-align: left;
    }

    .Footer-Right {
      position: relative;

      display: flex;
      align-items: flex-end;
      .DataView-Share-Opener {
        cursor: pointer;
        margin-right: 6px;
        > svg {
          width: auto !important;
        }
      }
      .DataView-Share-Buttons {
        position: absolute;
        padding: 8px;
        right: -4px;
        bottom: 1.5em;
        width: 240px;
        border: solid 1px $gray-4;
        background: $white !important;
        border-radius: 8px;
        text-align: left;
        font-size: 1rem;
        z-index: 1;

        > * {
          padding: 4px 0px;
        }

        > .Close-Button {
          display: flex;
          justify-content: flex-end;
          color: $gray-3;
        }

        > .EmbedCode {
          position: relative;
          padding: 4px;
          padding-right: 30px;
          color: rgb(3, 3, 3);
          border: solid 1px #eee;
          border-radius: 8px;

          font-size: 12px;

          .EmbedCode-Copy {
            position: absolute;
            top: 0.3em;
            right: 0.3em;
            color: $gray-3;
          }
        }

        > .Buttons {
          display: flex;
          justify-content: center;
          margin-top: 4px;

          .icon-resize {
            border-radius: 50%;
            width: 30px;
            height: 30px;
            font-size: 30px;
            margin-left: 4px;
            margin-right: 4px;

            &.twitter {
              color: #fff;
              background: #2a96eb;
            }
            &.facebook {
              color: #364e8a;
            }
            &.line {
              color: #1cb127;
            }
          }
        }
      }
    }
  }

  .overlay {
    position: absolute;
    display: flex;
    align-items: center;
    justify-content: center;
    z-index: 1;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    user-select: none;
    opacity: 0.8;

    > .overlay-text {
      text-align: center;
      padding: 2em;
      width: 60%;
      background: $gray-2;
      border-radius: 8px;
      color: $white !important;
    }
  }
}

textarea {
  font: 400 11px system-ui;
  width: 100%;
  height: 2.4rem;
}
</style>
