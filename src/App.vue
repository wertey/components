<template>
  <div id="app">
    <div>
      <h2>Dropdown</h2>
      <dropdown :dropdownOptions="dropdownOptions"/>
    </div>
    <div>
      <h2>Button</h2>
      <my-button :buttonOptions="buttonOptions"/>
    </div>
    <cookieNotification v-if="!cookieToken" :cookieToken.sync="cookieToken"/>
    <accordion  :items="items"/>
    <textInput
      :typeInput="'text'"
      :autofocus="true"
      :value.sync="newTypeService"
      @test="test"
    />
    <breadScrums :items="breadCrumbs" />
    <h2>Tabs</h2>
    <div>
      selected={{ selected }}
    </div>
    <tabNav :tabs="tabs" :selected="selected" @selected="setSelected">
      <tabItem v-if="selected === 'Home'">
        <p>Some text 1</p>
      </tabItem>
      <tabItem v-else-if="selected === 'Settings'">
        <h1>Large title of tab</h1>
      </tabItem>
      <tabItem v-else-if="selected === 'Profile'">
        <ul>
          <li>12</li>
          <li>13</li>
          <li>14</li>
          <li>15</li>
          <li>16</li>
        </ul>
      </tabItem>
    </tabNav>
    <div class="list">
      <div v-for="item in list" :key="item.id" class="list__item" :ref="item.id">
        {{ item.text }}
        <i @click="openModal($event, item.id)">x</i>
      </div>
    </div>
    <div ref="list" v-if="isVisibleModal && activeModal !== -1" class="modal">
      {{  activeModal  }}
    </div>
  </div>
</template>

<script>
import dropdown from './components/elements/dropdown.vue';
import accordion from './components/elements/accordion.vue';
import textInput from './components/elements/textInput.vue';
import cookieNotification from './components/cookieNotification.vue';
import breadScrums from './components/breadscrums.vue';
import buttonComponent from './components/elements/buttonComponent.vue';
import tabNav from './components/tab/tabNav.vue';
import tabItem from './components/tab/tab.vue';

export default {
  name: 'App',
  components: {
    dropdown,
    cookieNotification,
    textInput,
    accordion,
    breadScrums,
    tabNav,
    tabItem,
    myButton: buttonComponent,
  },
  data: () => ({
    activeModal: -1,
    isVisibleModal: false,
    newTypeService: '',
    cookieToken: false,
    items: [{
      id: 1,
      title: 'Competition law',
      content: 'wer 1Schärer Attorneys at Law advises and represents you on questions of unfair competition and the anti-trust law, for example, for company mergers, anti-trust investigations and for the drafting of distribution agreements.',
    }, {
      id: 2,
      title: 'Constitutional, community and administrative law',
      content: 'test 2 Civil law regulates privities of contract between private persons, communities of persons and corporations. On the other hand, constitutional, community and administrative law is concerned with the legal relationship between a private person and the community sector (federation, cantons, communities, associations of communities), or amongst communities. The specialists at Schärer Attorneys at Law act as advisers and consultants for private persons as well as communities, and represent them in the legal proceedings of objection and appeal.',
    },
    {
      id: 3,
      title: 'Construction, planning and environmental law',
      content: 'greee 3Our specialists in the fields of construction, planning and environmental law advise and represent builders, planners and architects, corporations, affected neighboring communities and associations of communities in:',
    }],
    tabs: ['Home', 'Settings', 'Profile'],
    selected: 'Home',
    list: [
      {
        id: 1,
        text: '123k123k12j31j23kj1231231',
      },
      {
        id: 2,
        text: '123k123k12j31j23kj1231231',
      },
      {
        id: 3,
        text: '123k123k12j31j23kj1231231',
      },
      {
        id: 4,
        text: '123k123k12j31j23kj1231231',
      },
      {
        id: 5,
        text: '123k123k12j31j23kj1231231',
      },
    ],
  }),
  computed: {
    breadCrumbs() {
      return [
        { name: '0000', route: '/' },
        { name: '123', route: '/selection-service' },
      ];
    },
    dropdownOptions() {
      return {
        list: [
          {
            code: 'Ru',
            text: 'Russian',
          },
          {
            code: 'En',
            text: 'English',
          },
        ],
      };
    },
    buttonOptions() {
      return {
        type: 'success',
        buttonText: 'Click me',
        disabled: false,
        onClick: this.test,
        icon: 'test',
      };
    },
  },
  methods: {
    openModal(e, id) {
      const el = e.currentTarget;
      console.log('el', el);
      this.isVisibleModal = true;
      if (this.activeModal !== id) this.activeModal = id;
      else this.activeModal = -1;
      console.log('id', id);
      this.$nextTick(() => {
        const left = el.getBoundingClientRect().x + 5;
        const top = el.getBoundingClientRect().y + 41;
        this.$refs.list.style.left = `${left}px`;
        this.$refs.list.style.top = `${top}px`;
      });
    },
    test() {
      console.log('clicked button');
    },
    setSelected(tab) {
      this.selected = tab;
    },
  },
  beforeMount() {
    const cookie = JSON.parse(localStorage.getItem('cookie'));
    if (cookie) {
      this.cookieToken = true;
    } else {
      this.cookieToken = false;
    }
  },
};
</script>

<style lang="scss">
  @import "sass/core";

  .test {
    width: 50px;
    height: 50px;
    background: red;
    margin: 0 15px;
  }

  .list {
    width: 100%;

    &__item {
      height: 50px;
      position: relative;
    }
  }

  .modal {
    position: absolute;
    width: 100px;
    height: 100px;
    background: red;
  }

  .vueperslides__track-inner {
    align-items: center;
  }

  .vueperslide.test.vueperslide--active.vueperslide--visible {
    background: green;
    width: 70px;
    height: 70px;
  }
</style>
