<script setup>
import { ref } from 'vue';

const GW2_API = 'https://api.guildwars2.com/v2';
const LANG = 'fr';

const apiKey = ref(null)
const characters = ref(null);
const error = ref(null);
const userQuests = ref({});
const quests = ref({});
const questsOrder = {
  15: 0,
  77: 0,
  117: 0,
  154: 0,
  201: 0,

  25: 1,
  17: 1,
  21: 1,
  60: 1,
  71: 1,
  59: 1,
  124: 1,
  123: 1,
  115: 1,
  155: 1,
  159: 1,
  162: 1,
  202: 1,
  203: 1,
  212: 1,

  28: 2,
  16: 2,
  24: 2,
  61: 2,
  72: 2,
  62: 2,
  116: 2,
  109: 2,
  118: 2,
  157: 2,
  163: 2,
  164: 2,
  206: 2,
  205: 2,
  213: 2,

  29: 3,
  22: 3,
  23: 3,
  63: 3,
  65: 3,
  74: 3,
  73: 3,
  64: 3,
  113: 3,
  106: 3,
  119: 3,
  158: 3,
  165: 3,
  169: 3,
  168: 3,
  204: 3,
  208: 3,
  214: 3,

  31: 4,
  30: 4,
  18: 4,
  26: 4,
  32: 4,
  66: 4,
  75: 4,
  67: 4,
  111: 4,
  112: 4,
  107: 4,
  108: 4,
  121: 4,
  120: 4,
  160: 4,
  161: 4,
  167: 4,
  166: 4,
  170: 4,
  210: 4,
  207: 4,
  218: 4,
  211: 4,
  215: 4,

  33: 5,
  19: 5,
  20: 5,
  27: 5,
  68: 5,
  76: 5,
  70: 5,
  69: 5,
  114: 5,
  110: 5,
  122: 5,
  156: 5,
  172: 5,
  171: 5,
  209: 5,
  219: 5,
  217: 5,
  216: 5,

  39: 6,
  44: 6,
  34: 6,
  78: 6,
  83: 6,
  88: 6,
  129: 6,
  125: 6,
  134: 6,
  173: 6,
  175: 6,
  184: 6,
  230: 6,
  220: 6,
  225: 6,

  40: 7,
  45: 7,
  35: 7,
  79: 7,
  84: 7,
  89: 7,
  130: 7,
  127: 7,
  126: 7,
  135: 7,
  174: 7,
  177: 7,
  178: 7,
  183: 7,
  231: 7,
  221: 7,
  226: 7,

  41: 8,
  43: 8,
  46: 8,
  48: 8,
  36: 8,
  37: 8,
  81: 8,
  80: 8,
  85: 8,
  90: 8,
  131: 8,
  132: 8,
  128: 8,
  137: 8,
  136: 8,
  182: 8,
  176: 8,
  179: 8,
  185: 8,
  232: 8,
  222: 8,
  223: 8,
  227: 8,
  228: 8,

  42: 9,
  47: 9,
  38: 9,
  82: 9,
  86: 9,
  87: 9,
  91: 9,
  92: 9,
  133: 9,
  139: 9,
  138: 9,
  181: 9,
  180: 9,
  186: 9,
  187: 9,
  233: 9,
  234: 9,
  224: 9,
  229: 9,

  49: 10,
  93: 10,
  140: 10,
  188: 10,
  236: 10,

  50: 11,
  51: 11,
  94: 11,
  95: 11,
  141: 11,
  142: 11,
  189: 11,
  197: 11,
  237: 11,
  239: 11,

  56: 12,
  57: 12,
  97: 12,
  98: 12,
  145: 12,
  144: 12,
  194: 12,
  191: 12,
  244: 12,
  238: 12,

  52: 13,
  53: 13,
  100: 13,
  101: 13,
  147: 13,
  148: 13,
  192: 13,
  195: 13,
  235: 13,
  241: 13,

  54: 14,
  55: 14,
  58: 14,
  104: 14,
  105: 14,
  102: 14,
  151: 14,
  152: 14,
  150: 14,
  200: 14,
  199: 14,
  198: 14,
  247: 14,
  243: 14,
  246: 14,

  358: 15,

  251: 16,
  253: 16,
  248: 16,

  256: 17,
  255: 17,
  249: 17,

  261: 18,
  254: 18,
  258: 18,
  259: 18,
  250: 18,

  257: 19,
  260: 19,
  262: 19,
  252: 19,

  263: 20,
  267: 20,
  271: 20,
  274: 20,
  278: 20,

  264: 21,
  266: 21,
  268: 21,
  270: 21,
  272: 21,
  275: 21,
  276: 21,
  279: 21,
  280: 21,

  265: 22,
  269: 22,
  273: 22,
  277: 22,
  281: 22,

  285: 23,
  290: 23,
  295: 23,

  282: 24,

  286: 25,
  291: 25,
  296: 25,

  288: 26,
  287: 26,
  292: 26,
  293: 26,
  298: 26,
  297: 26,

  283: 27,

  289: 28,
  294: 28,
  299: 28,

  284: 29,

  301: 30,

  309: 31,
  304: 31,
  312: 31,

  307: 32,
  308: 32,
  303: 32,
  302: 32,
  313: 32,

  310: 33,
  305: 33,
  314: 33,
  315: 33,

  300: 34,

  311: 35,
  306: 35,
  316: 35,

  321: 36,

  335: 37,
  334: 37,

  320: 38,

  337: 39,
  336: 39,

  339: 40,
  338: 40,

  318: 41,

  325: 42,
  331: 42,
  328: 42,

  326: 43,
  332: 43,
  329: 43,

  327: 44,
  333: 44,
  330: 44,

  322: 45,

  317: 46,
  319: 46,

  323: 47,

  324: 48,

  // Remembering Scarlet's War (résumé)
  415: 49,

  // S01E01 : +5
  // S01E02 : +6
  // S01E03 : +5
  // S01E04 : +6
  // S01E05 : +8

  // S02E01
  363: 80,
  362: 81,
  360: 82,
  359: 83,
  361: 84,

  // S02E02
  364: 84,
  365: 85,
  366: 87,
  368: 88,
  367: 89,

  // S02E03
  371: 90,
  372: 91,
  369: 92,
  374: 93,
  373: 94,

  // S02E04
  378: 95,
  377: 96,
  379: 97,
  375: 98,

  // S02E05
  384: 99,
  381: 100,
  383: 101,
  380: 102,

  // S02E06
  386: 103,
  387: 104,
  385: 105,

  // S02E07
  388: 106,
  389: 107,
  390: 108,

  // S02E08
  391: 109,
  392: 110,
  393: 111,

  // HoT1
  411: 112,
  409: 113,
  419: 114,
  405: 115,
  421: 116,
  410: 117,

  // HoT2
  407: 118,
  402: 119,
  423: 120,
  413: 121,

  // HoT3
  408: 122,
  417: 123,
  401: 124,
  418: 125,

  // HoT4
  398: 126,
  404: 127,

  // S03E01
  431: 128,
  435: 129,
  432: 130,
  426: 131,
  429: 132,
  425: 133,

  // S03E02
  441: 134,
  443: 135,
  440: 136,
  436: 137,
  442: 138,
  437: 139,
  444: 140,

  // S03E03
  453: 141,
  447: 142,
  452: 143,
  448: 144,
  449: 145,
  451: 146,
  446: 147,

  // S03E04
  456: 148,
  458: 149,
  457: 150,
  459: 151,
  455: 152,
  454: 153,

  // S03E05
  466: 154,
  461: 155,
  460: 156,
  462: 157,
  464: 158,

  //S03E06
  469: 159,
  473: 160,
  475: 161,
  472: 162,
  468: 163,

  // PoF1
  488: 164,
  503: 165,
  480: 166,
  486: 167,
  482: 168,
  491: 169,

  // PoF2
  484: 170,
  500: 171,
  493: 172,
  477: 173,

  // PoF3
  496: 174,
  479: 175,
  501: 176,
  483: 177,
  481: 178,
  499: 179,

  // S04E01
  505: 180,
  506: 181,
  509: 182,
  511: 183,
  504: 184,
  508: 185,

  // S04E02
  526: 186,
  525: 187,
  521: 188,
  519: 189,
  513: 190,

  // S04E03
  529: 191,
  527: 192,
  533: 193,
  535: 194,
  532: 195,

  // S04E04
  537: 196,
  536: 197,
  538: 198,
  540: 199,
  539: 200,

  // S04E05
  543: 201,
  544: 202,
  545: 203,
  546: 204,
  542: 205,

  // S04E06
  549: 206,
  552: 207,
  548: 208,
  551: 209,
  547: 210,

  // IBS0
  553: 211,
  554: 212,
  557: 213,
  556: 214,
  555: 215,

  // IBS1
  562: 216,
  559: 217,
  563: 218,
  561: 219,

  // IBS2
  564: 220,
  568: 221,
  567: 222,
  566: 223,
  565: 224,

  // IBS3
  570: 225,
  569: 226,

  // IBS4
  571: 227,
  574: 228,
  573: 229,
  572: 230,
  575: 231,

  // IBS5
  576: 232,
  577: 233,
  579: 234,
  578: 235,
  580: 236,

  // IBS6
  595: 237,
  596: 238,
  583: 239,

  // IBS7
  581: 240,
  605: 241,

  // IBS8
  608: 242,
  599: 243,
  593: 244,
  590: 245,

  // ISB9
  611: 246,
  598: 247,
};
const questExcluded = [587, 600, 602, 606];  // Calm in the Storm (placeholder between lasts IBS episodes)
const questsOrderKeys = [
  5,
  15,
  24,
  105,
  122
];

initQuests();

function initQuests() {
  fetch(`${GW2_API}/quests`)
    .then((res) => res.json())
    .then((ids) => {
      const chunkSize = 200;
      for (let i = 0; i < ids.length; i += chunkSize) {
        const chunk = ids.slice(i, i + chunkSize);
        const _ids = chunk.join(',');
        fetch(`${GW2_API}/quests?ids=${_ids}&lang=${LANG}`)
          .then((res) => res.json())
          .then(json => {
            json.forEach(q => {
              quests.value[q.id] = q;
            })
          })
      }
    })
    .catch((err) => (error.value = err))
}

function loadCharacters() {
  error.value = null;
  fetch(`${GW2_API}/characters?access_token=${apiKey.value}`)
    .then((res) => res.json())
    .then((json) => {
      characters.value = json;
      json.forEach(c => {
        fetch(`${GW2_API}/characters/${c}/quests?access_token=${apiKey.value}`)
          .then((res) => res.json())
          .then((json) => {
            let uQuests = [];
            for (let i = 0; i <= 247; i++) {
              uQuests[i] = 0;
            }
            json.forEach((j) => {
              if (typeof questsOrder[j] !== 'undefined' && questExcluded.indexOf(j) < 0) {
                uQuests[questsOrder[j]] = j;
              } else if (questExcluded.indexOf(j) >= 0) {
                console.log('!!excluded!!', 'char:', c, 'quest:', j);
              } else {
                console.log('!!undefined!!', 'char:', c, 'quest:', j);
              }
            })
            userQuests.value[c] = uQuests;
          })
          .catch((err) => (error.value = err))
      });
    })
    .catch((err) => (error.value = err))
}

function getSeasonClass(q) {

  const o = q;

  if (o <= 48) {
    return 'story--s00';
  } else if (o >= 49 && o <= 79) {
    return 'story--s01';
  } else if (o >= 80 && o <= 111) {
    return 'story--s02';
  } else if (o >= 112 && o <= 127) {
    return 'story--hot';
  } else if (o >= 128 && o <= 163) {
    return 'story--s03';
  } else if (o >= 164 && o <= 179) {
    return 'story--pof';
  } else if (o >= 180 && o <= 210) {
    return 'story--s04';
  } else if (o >= 211 && o <= 247) {
    return 'story--ibs';
  }

  return 'story--xxx';

}

function getQuestOrderKeyClass(i) {
  if (questsOrderKeys.indexOf(i) >= 0) {
    return 'story--key';
  }

  return '';
}

</script>

<template>
  <h1 class="container mx-auto my-6 text-3xl font-semibold">Histoire de mes personnages</h1>
  <div v-if="error" class="container mx-auto my-6">
    <div class="alert alert-error shadow-lg">
      <div>
        <svg xmlns="http://www.w3.org/2000/svg" class="stroke-current flex-shrink-0 h-6 w-6" fill="none"
          viewBox="0 0 24 24">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
            d="M10 14l2-2m0 0l2-2m-2 2l-2-2m2 2l2 2m7-2a9 9 0 11-18 0 9 9 0 0118 0z" />
        </svg>
        <span>Error: {{ JSON.stringify(error) }}</span>
      </div>
    </div>
  </div>
  <div class="container mx-auto my-6 flex gap-4 items-center">
    <input type="text" placeholder="Clé API Guild Wars 2" v-model="apiKey" class="input input-bordered w-full" />
    <button @click="loadCharacters" class="btn">Charger</button>
  </div>
  <div class="container mx-auto my-6">
    <div v-for="(uq, c) in userQuests">
      <h2 class="mb-4">
        <label :for="c" class="btn btn-outline gap-2">
          <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor"
            class="w-6 h-6">
            <path stroke-linecap="round" stroke-linejoin="round"
              d="M2.036 12.322a1.012 1.012 0 010-.639C3.423 7.51 7.36 4.5 12 4.5c4.638 0 8.573 3.007 9.963 7.178.07.207.07.431 0 .639C20.577 16.49 16.64 19.5 12 19.5c-4.638 0-8.573-3.007-9.963-7.178z" />
            <path stroke-linecap="round" stroke-linejoin="round" d="M15 12a3 3 0 11-6 0 3 3 0 016 0z" />
          </svg>
          {{ c }}
        </label>
      </h2>
      <ul class="flex flex-wrap gap-2 mb-8">
        <li v-for="(q, i) in uq" :key="c + q + i" class="story tooltip"
          :data-tip="(typeof quests[q] !== 'undefined') ? quests[q].name : null"
          :class="getSeasonClass(i) + ' ' + ((q > 0) ? 'story--done' : '') + ' ' + getQuestOrderKeyClass(i)"
          :data-quest="q">
          <span></span>
        </li>
      </ul>
    </div>
  </div>

  <div v-for="(uq, c) in userQuests">
    <input type="checkbox" :id="c" class="modal-toggle" />
    <label :for="c" class="modal cursor-pointer">
      <label class="modal-box relative rounded-none" for="">
        <h3 class="text-2xl font-bold">{{ c }}</h3>
        <div class="my-4" v-for="q in uq">
          <div v-if="(typeof quests[q] !== 'undefined')">
            <h4 class="text-lg font-bold mb-2 text-gray-500">{{ quests[q].name }}</h4>
            <ul v-if="quests[q].goals">
              <li v-for="goal in quests[q].goals" class="mb-4">
                <div v-if="goal.active" class="font-semibold italic mb-2">{{ goal.active }}</div>
                <div v-if="goal.complete">{{ goal.complete }}</div>
              </li>
            </ul>
          </div>
        </div>
      </label>
    </label>
  </div>
</template>

<style scoped>
.story {
  @apply bg-gray-200 text-white block aspect-square w-8 opacity-30 rounded;
}

.story>span:before {
  @apply w-full h-full flex items-center justify-center text-xs font-bold absolute top-0 left-0;
}

.story--done {
  @apply opacity-100;
}

.story--key {
  @apply ring ring-offset-base-100 ring-offset-2;
}

.story--s00,
.story--s01,
.story--s02 {
  @apply bg-red-600;
}

.story--s00.story--key,
.story--s01.story--key,
.story--s02.story--key {
  @apply ring-red-600;
}

.story--s00>span:before {
  content: 'HP';
}

.story--s01>span:before {
  content: 'S01';
}

.story--s02>span:before {
  content: 'S02';
}

.story--s03,
.story--hot {
  @apply bg-green-600;
}

.story--s03.story--key,
.story--hot.story--key {
  @apply ring-green-600;
}

.story--s03>span:before {
  content: 'S03';
}

.story--hot>span:before {
  content: 'HoT';
}

.story--pof,
.story--s04 {
  @apply bg-rose-500;
}

.story--pof.story--key,
.story--s04.story--key {
  @apply ring-rose-500;
}

.story--pof>span:before {
  content: 'PoF';
}

.story--s04>span:before {
  content: 'S04';
}

.story--ibs {
  @apply bg-blue-600;
}

.story--ibs.story--key {
  @apply ring-blue-600;
}

.story--ibs>span:before {
  content: 'ÉdG';
}
</style>
