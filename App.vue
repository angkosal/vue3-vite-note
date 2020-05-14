<template>
  <div class="main columns is-marginless">
    <div class="column is-narrow sidebar is-paddingless">
      <div style="width: 300px;">
        <button @click="handleClickAdd" class="button is-fullwidth is-primary">
          New
        </button>
        <ul>
          <li v-for="(note, index) in notes">
            <a
              @click="handleClickNote(index)"
              class="item"
              :class="{ active: index === currentIndex }"
              href="#"
              :title="`${note.title} (${formatDate(note.created_at)})`"
              >{{ note.title }}</a
            >
          </li>
        </ul>
      </div>
    </div>
    <div class="column content is-paddingless" style="">
      <input
        v-model="note.title"
        ref="title"
        type="text"
        class="note-title"
        placeholder="Title"
        @keyup="handleChange('title')"
      /><textarea
        class="note-content"
        placeholder="Content..."
        v-model="note.content"
        @keyup="handleChange('content')"
      ></textarea
      ><button @click="handleClickDelete" class="btnDelete button is-danger" type="button">Delete</button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'App',
  components: {},
  data() {
    return {
      notes: [],
      note: {},
      currentIndex: 0,
    };
  },
  mounted() {
    this.notes = JSON.parse(window.localStorage.getItem('notes')) || [];
    if (this.notes.length) {
      this.note = { ...this.notes[0] };
    }
  },
  methods: {
    handleClickNote(index) {
      this.currentIndex = index;
      this.note = { ...this.notes[this.currentIndex] };
    },
    handleClickAdd() {
      this.add({
        title: 'Title',
        content: '',
        created_at: new Date(),
      });
      window.localStorage.setItem('notes', JSON.stringify(this.notes));
      setTimeout(() => {
        this.$refs.title.select();
      });
    },
    add(note) {
      this.notes.unshift(note);
      this.note = { ...note };
    },
    handleChange(field) {
      this.notes[this.currentIndex][field] = this.note[field];
      window.localStorage.setItem('notes', JSON.stringify(this.notes));
    },
    formatDate(currentDate) {
      const date = new Date(currentDate);
      return (
        date.getFullYear() + '-' + (date.getMonth() + 1) + '-' + date.getDate()
        + ' ' + date.getHours() + ':' + date.getMinutes() + ':' + date.getSeconds()
      )
    },
    handleClickDelete() {
      this.notes.splice(this.currentIndex, 1);
      this.currentIndex = 0;
      this.note = {...this.notes[this.currentIndex]}
      window.localStorage.setItem('notes', JSON.stringify(this.notes));
    }
  },
};
</script>
