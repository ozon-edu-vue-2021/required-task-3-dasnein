<template>
  <div class="person">
    <div class="person__photo">
      <img :src="person.picture" alt="photo" />
    </div>
    <div class="person__info person__info-right">
      <div class="person__info-name">
        <b>{{ person.name }} ({{ person.age }})</b>
      </div>

      <div class="person__info-email">
        <p>
          <b>Почта:</b><br /><a :href="`mailto:${person.email}`">{{
            person.email
          }}</a>
        </p>
      </div>
      <div class="person__info-email">
        <p><b>Дата регистрации:</b><br />{{ formatedDate }}</p>
      </div>
    </div>
    <div class="person__info">
      <div class="person__info-about">
        <p><b>О себе:</b><br />{{ person.about }}</p>
      </div>
    </div>
  </div>
</template>

<script>
import { DateTime } from "luxon";

export default {
  props: {
    person: {
      type: Object,
      default: null,
    },
  },

  computed: {
    formatedDate() {
      return DateTime.fromISO(this.person.registered).toLocaleString();
    },
  },
};
</script>

<style scoped>
.person {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
}

.person__photo {
  width: 30%;
}

.person__photo img {
  width: 100%;
  height: auto;
}

.person__info {
  margin-top: 10px;
  line-height: 1.2em;
}

.person__info-right {
  width: 64%;
}

.person__info-name {
  margin-bottom: 10px;
  font-size: 1.5em;
}
</style>
