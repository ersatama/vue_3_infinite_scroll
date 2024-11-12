<script>
/** Documentation infinite scroll */
export default {}
</script>
<script setup lang="ts">
import {onMounted, ref} from 'vue';


/**
 * This fetch API interface represents the response to a request https://randomuser.me/api/
 */
interface Api {
  readonly results: any
}

/**
 * This interface represents data taken from the response https://randomuser.me/api/
 */
interface User {
  readonly picture: {
    readonly medium: string
  },
  readonly name: {
    readonly title: string,
    readonly first: string,
    readonly last: string,
  },
  readonly email: string
}

/**
 * This interface represents ths structure of a Card
 */
interface Card {
  readonly img: string,
  readonly name: string,
  readonly email: string
}

/**
 * List of cards
 * @type {Ref<{img: String, name: String, email: String}[]>}
 */
const cards  = ref<any>([]);

/**
 * Pagination for api request
 * @type {Ref<number>}
 */
const page = ref<number>(1);

/**
 * Limit of records per api request
 * @type {Ref<number>}
 */
const take = ref<number>(25);

/**
 * API request status. API will send request only if status is true.
 * @type {Ref<boolean>}
 */
const status = ref<boolean>(true);

/**
 * API function to get data from https://randomuser.me/api/
 * @example
 * ```
 * update()
 * ```
 */
const update = async (): Promise<void> => {

  /** Check if status is true */
  if (status.value) {

    /** Set status to false */
    status.value = false;

    /**
     * @description Raw data which we got from the res-api
     * it's able to convert to JSON
     */
    fetch('https://randomuser.me/api/?page=' + page.value + '&results=' + take.value)
        .then(response => response.json())
        .then((data: Api) => {

          /** Update page to the next page */
          page.value++;

          /** Add new data fro api to existing data*/
          cards.value = cards.value.concat(data.results.map((user: User): Card => {
            return {
              img: user.picture.medium,
              name: user.name.title + ' ' + user.name.first + ' ' + user.name.last,
              email: user.email
            };
          }));

        })
        .catch(error => console.error('something goes wrong!', error))
        .finally(() => {

          /** Set status to true */
          status.value = true;

        });
  }
}


onMounted(() => {

  /** Init update() function to get data https://randomuser.me/api/*/
  update();

  /** Listen scroll event and get new data from https://randomuser.me/api/*/
  window.onscroll = () => {

    /** Check if client scrolled to bottom of the page*/
    if ((window.document.documentElement.scrollTop + window.document.documentElement.clientHeight)  >= (window.document.documentElement.scrollHeight - 400)) {
      update();
    }
  }

})

/** Component for cards list view*/
import Cards from './components/Cards.vue';

/** Component for card view*/
import Card from './components/Card.vue';
</script>

<template>
  <cards>
    <card v-for="(card, key) in cards" :key="key" :card="card"></card>
  </cards>
</template>

<style lang="scss">

</style>
