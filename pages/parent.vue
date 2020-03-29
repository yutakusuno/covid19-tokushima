<template>
  <div class="Parent">
    <h2 class="Parent-Heading">
      {{ $t('臨時休校中の新型コロナウイルス感染症対応についてのお願い') }}
    </h2>
    <StaticCard>
      <h3>1. {{ $t('感染予防・健康管理') }}</h3>
      <ul>
        <li>
          {{
            $t(
              '不特定多数の人の集まる場所等への外出を避け、基本的に自宅で過ごしてください。'
            )
          }}
        </li>
        <li>
          {{ $t('手洗い、咳エチケット等により、感染予防に努めてください。')
          }}<br />
          <a
            href="https://tokyodouga.jp/lViN9C_BS-0.html"
            target="_blank"
            rel="noopener"
            >{{ $t('【参考】感染症予防のための正しい手洗い方法（動画）') }}</a
          >
        </li>
        <li>
          {{
            $t(
              '規則正しい生活を心がけ、日常の健康管理に十分気を付けてください。'
            )
          }}
        </li>
      </ul>
    </StaticCard>
    <StaticCard>
      <h3>2. {{ $t('感染症を疑う場合の対応') }}</h3>
      <ul>
        <li>{{ $t('各保健所にご相談ください') }}</li>
        <li>
          {{ $t('「新型コロナウイルス感染症にかかる相談窓口について」') }}<br />
          <table summary="相談窓口（帰国者・接触者相談センター）連絡先一覧">
            <tbody>
              <tr>
                <th>
                  <strong>相談窓口</strong>
                </th>
                <th>
                  <strong>電話番号</strong>
                </th>
                <th>
                  <strong>FAX（土日祝除く）</strong>
                </th>
                <th>
                  <strong>開設時間</strong>
                </th>
              </tr>
              <tr>
                <td>徳島保健所</td>
                <td>088－602－8907</td>
                <td>088－652－9334</td>
                <td>8時30分～17時15分</td>
              </tr>
              <tr>
                <td>吉野川保健所</td>
                <td>088－336－9018</td>
                <td>088－322－1760</td>
                <td>8時30分～17時15分</td>
              </tr>
              <tr>
                <td>阿南保健所</td>
                <td>088－428－9874</td>
                <td>088－422－6404</td>
                <td>8時30分～17時15分</td>
              </tr>
              <tr>
                <td>美波保健所</td>
                <td>088－474－7373</td>
                <td>088－474－7365</td>
                <td>8時30分～17時15分</td>
              </tr>
              <tr>
                <td>美馬保健所</td>
                <td>088－352－1016</td>
                <td>088－353－9446</td>
                <td>8時30分～17時15分</td>
              </tr>
              <tr>
                <td>三好保健所</td>
                <td>088－372－1123</td>
                <td>088－372－6884</td>
                <td>8時30分～17時15分</td>
              </tr>
            </tbody>
          </table>
        </li>
      </ul>
    </StaticCard>
    <StaticCard>
      <h3>3. {{ $t('その他') }}</h3>
      <p>{{ $t('詳細は、各学校からのお知らせ等をご確認ください。') }}</p>
    </StaticCard>
  </div>
</template>

<script lang="ts">
import Vue from 'vue'
import { MetaInfo } from 'vue-meta'
import StaticCard from '@/components/StaticCard.vue'

type ItemData = {
  items: Item[]
}

type Item = {
  title: string
  body: string
}

export default Vue.extend({
  components: {
    StaticCard
  },
  head: (): MetaInfo => ({
    title: 'お子様をお持ちの皆様へ'
  })
})
</script>

<style lang="scss">
.Parent {
  &-Heading {
    @include font-size(30);
    font-weight: normal;
    color: $gray-2;
    margin-bottom: 12px;
  }
}

th {
  text-align: center;
}

td {
  padding: 5px 15px;
}
</style>
