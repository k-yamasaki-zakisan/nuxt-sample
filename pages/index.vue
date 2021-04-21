<template>
  <sectiom class="container">
    <h1>ToDoリスト</h1>
    <div class="addArea">
      <input type="text" name="addName" v-model="content" placeholder="タスクを入力してください" />
      <button id="addButton" class="button button--green" @click="insert">追加</button>
    </div>
    <div class="Filter">
      <button
        class="button button--gray"
        v-bind:class="{'is-active':(!find_flg)}"
        @click="flag_reset"
      >全て</button>
      <button
        class="button button--gray"
        v-bind:class="{'is-active':find_flg && (find_state == '作業前')}"
        @click="find('作業前')"
      >作業前</button>
      <button
        class="button button--gray"
        v-bind:class="{'is-active':find_flg && (find_state == '作業中')}"
        @click="find('作業中')"
      >作業中</button>
      <button
        class="button button--gray"
        v-bind:class="{'is-active':find_flg && (find_state == '完了')}"
        @click="find('完了')"
      >完了</button>
    </div>
    <table class="Lists">
      <thead>
        <tr>
          <th>タスク</th>
          <th>登録日時</th>
          <th>状態</th>
          <th></th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(item, index) in display_todos" :key="index">
          <td>{{ item.content }}</td>
          <td>{{ item.created }}</td>
          <td>
            <button
              class="button"
              v-bind:class="{
                'button--yet':item.state == '作業前',
                'button--progress':item.state == '作業中',
                'button--done':item.state == '完了'
              }"
              @click="changeState(item)"
            >{{ item.state }}</button>
          </td>
          <td>
            <button class="button button--delete" @click="remove(item)">削除</button>
          </td>
        </tr>
      </tbody>
    </table>
  </sectiom>
</template>

<script lang="ts">
// import Vue from "vue";

// export default Vue.extend({});
import { mapState } from "vuex";
import createStore from "../store/index";

export type DataType = {
  content: string;
  find_state: string;
  find_flg: boolean;
};

export default {
  data(): DataType {
    return {
      content: "",
      find_state: "",
      find_flg: false
    };
  },
  computed: {
    ...mapState(["todos"]),
    display_todos(): any[] {
      // @ts-ignore
      if (this.find_flg) {
        let arr: any[] = [];
        let data: any = this.todos;

        for (let element of data) {
          // @ts-ignore
          if (element.state == this.find_state) {
            arr.push(element);
          }
        }
        return arr;
      } else {
        // @ts-ignore
        return this.todos;
      }
    },
    find_state_getter(): string {
      // @ts-ignore
      return this.find_state;
    }
  },
  methods: {
    insert(): void {
      // @ts-ignore
      if (this.content != "") {
        // @ts-ignore
        this.$store.dispatch("insert", { content: this.content });
        // this.$store.commit("insert", { content: this.content });
        // @ts-ignore
        this.content = "";
      }
    },
    remove(todo: any): void {
      // @ts-ignore
      this.$store.dispatch("remove", todo);
      //this.$store.commit("remove", todo);
    },
    changeState(todo: any): void {
      // @ts-ignore
      this.$store.dispatch("changeState", todo);
      //this.$store.commit("changeState", todo);
    },
    find(findState: string): void {
      // @ts-ignore
      this.find_state = findState;
      // @ts-ignore
      this.find_flg = true;
    },
    flag_reset(): void {
      (this as any).find_flg = false;
    }
  }
};
</script>

<style>
</style>
