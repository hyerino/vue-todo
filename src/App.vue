<script setup>
//  vue3 컴포지션 api 가져오기
// 객체구조분해로 필요한 함수 불러오기
import { computed, reactive } from 'vue';

// reactive함수 : 객체 데이터에 반응성을 주입하여 값이 변경되면 rerendering이 되어 화면이 갱신됨
// * 반응성 : 데이터가 변경되면 화면이 바로 갱신되는 반응형 시스템
// re-rendering : 사용자가 화면에 뷰를 다시 새롭게 보여주는 것을 의미함
const data = reactive({
  newItem: '',
  items: [],
});

// 할 일 완료 요소의 배열 길이 구하기
// let이 아닌 const인 이유 : rendering 간에 새로운 변수가 생성되며 변경된 값이 변수에 각각 할당됨
// 완료된 요소를 새로운 배열로 만들고 갯수를 구해줌
const isComplete = computed(
  () => data.items.filter((item) => item.completed === true).length
);

// computed 내부에 콜백함수를 익명함수로 부름
// computed 함수 : data.items가 변경될 때만 함수를 실행(결과를 캐싱함)
const totalItems = computed(() => data.items.length);

// 데이터 추가
// 할 일 객체 데이터는 고유 id를 추가하여 데이터를 구분할 수 있도록 해야함
function addItem() {
  data.items.push({
    id: data.items.length,
    text: data.newItem,
    completed: false,
  });
  // 데이터 초기화
  data.newItem = '';
}

// 할 일 삭제
function deleteItem(id) {
  console.log(id);
  // id에 맞는 아이템ㅇ르 찾기
  const itemToDelete = data.items.find((item) => item.id === id);
  console.log(itemToDelete);
  // 삭제할 아이템의 위치 찾기
  const idx = data.items.indexOf(itemToDelete);
  // splice(배열요소 시작위치, 삭제 갯수)
  data.items.splice(idx, 1);
}
function test1() {
  console.log(test1);
}
</script>

<template>
  <main class="app">
    <h1>Simple to-do list</h1>
    <!-- 자바스크립트 사용시 {{  }} 사용 -->
    <div class="todo_count">
      완료된 할 일: {{ isComplete }} / 할 일: {{ totalItems }}
    </div>
    <div class="todo_add">
      <!-- v-model : 데이터 양방향 연결
        => 양방향 연결 : 한 쪽의 데이터가 변경되면 다른 한 쪽도 동일하게 데이터 값이 변경됨 -->
      <input
        type="text"
        title="할 일을 입력하세요"
        placeholder="할 일을 입력하세요"
        v-on:keyup.enter="addItem()"
        v-model="data.newItem"
      />
      <!-- v-on : 이벤트명 => 이벤트 연결 디렉티브 -->
      <button type="button" class="add_btn" v-on:click="addItem()">Add</button>
    </div>
    <div class="todo_add">
      <ul class="todo_list">
        <!-- v-vind 디렉티브 : 데이터 단방향 연결 -->
        <!-- 리스트는 고유값을 key속성에 연결하여 순서가 변경돼도 구별 가능하도록 -->
        <!-- todo.completed가 true면 completed 문자열이 클래스 명이 됨 -->
        <li
          v-for="(todo, i) in data.items"
          v-bind:key="todo.id"
          v-bind:class="{ completed: todo.completed }"
        >
          <!-- id가 달라야 되므로id 속성과 todo.ker 값을 단방향으로 연결 -->
          <input
            type="checkbox"
            v-bind:id="`check${todo.id}`"
            v-model="todo.completed"
          />
          <!-- label클릭 시 for로 연결된 체크박스에서 true, false로 변경됨 -->
          <label v-bind:for="`check${todo.id}`">{{ todo.text }}</label>
          <button
            type="button"
            class="remove_btn"
            v-on:click="deleteItem(todo.id)"
          >
            Remove
          </button>
        </li>
      </ul>
    </div>
  </main>
</template>

<style scoped>
.app {
  padding: 40px;
}
.app h1 {
  font-size: 30px;
  font-weight: 700;
  margin-bottom: 20px;
}
.todo_count {
  margin-bottom: 10px;
  display: flex;
}
.todo_add input[type='text'] {
  width: calc(100% - 60px);
  border: 1px solid #ddd;
  border-radius: 4px;
}
.add_btn {
  padding: 0 10px;
  color: #fff;
  background: #333;
  height: 40px;
  border-radius: 4px;
  margin-left: 10px;
  border: none;
}
.todo_list {
  margin-top: 20px;
}
.todo_list li {
  display: flex;
  align-items: center;
  margin-bottom: 10px;
}
.todo_list li.completed label {
  text-decoration: line-through;
}
.todo_list label {
  width: 100%;
}
.remove_btn {
  height: 32px;
  padding: 0 10px;
  background: none;
  border: 1px solid var(--point-color1);
  color: var(--point-color1);
  border-radius: 4px;
  margin-left: 20px;
}
</style>
