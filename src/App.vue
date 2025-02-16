```vue
<template>
  <div id="app">
    <header class="header">
      <select v-model="selectedLocation" class="location-select" @change="clearTable">
        <option value="天西">天西</option>
        <option value="天东">天东</option>
        <option value="前门">前门</option>
      </select>
      <h1>{{ currentDate }} {{ selectedLocation }}排班表</h1>
    </header>
    <div class="schedule-list">
      <div class="time-row header-row">
        <span class="time-slot">时间</span>
        <span class="column-header">A/D</span>
        <span class="column-header">B/C</span>
      </div>
      <div class="time-row" v-for="(schedule, index) in schedules" :key="index">
        <span class="time-slot">{{ schedule.time }}</span>
        <input class="column-input" type="text" v-model="schedule.b" placeholder="A/D" @change="incrementUsage(schedule.b)" @click="setCurrentIndex(index, 'B')" />
        <input class="column-input" type="text" v-model="schedule.c" placeholder="B/C" @change="incrementUsage(schedule.c)" @click="setCurrentIndex(index, 'C')" />
      </div>
    </div>
  </div>
  <div class="tags">
      <span
        v-for="(person, index) in personnel"
        :key="index"
        class="tag"
        @click="fillColumn(person)"
        :class="{ 'disabled': personUsage[person] >= 6 }"
      >
        {{ person }} ({{ personUsage[person] }})
      </span>
  </div>
</template>


<script>
export default {
  name: 'App',
  data() {
    return {
      currentDate: '',
      selectedLocation: '天东',
      personnel: ['汪', '帆', '海', '娜', '杰', '昂', '扬', '尚', '俊', '巨', '亮'],
      schedules: [
        { time: '10-11', b: '', c: '' },
        { time: '11-12', b: '', c: '' },
        { time: '12-13', b: '', c: '' },
        { time: '13-14', b: '', c: '' },
        { time: '14-15', b: '', c: '' },
        { time: '15-16', b: '', c: '' },
        { time: '16-17', b: '', c: '' },
        { time: '17-18', b: '', c: '' },
        { time: '18-19', b: '', c: '' },
        { time: '19-20', b: '', c: '' },
        { time: '20-21', b: '', c: '' },
        { time: '21-22', b: '', c: '' },
        { time: '22-23', b: '', c: '' },
        // Special times
        { time: '5:30-7:00', b: '', c: '' },
        { time: '7:00-8:30', b: '', c: '' },
        { time: '8:30-10:00', b: '', c: '' },
        { time: '安检', b: '', c: '' },
        { time: '5314', b: '', c: '' },
        { time: '5317', b: '', c: '' },
        { time: '5318', b: '', c: '' }
      ],
      currentIndex: null,
      currentColumn: null, // 当前列
      personUsage: {
        '汪': 0, '帆': 0, '海': 0, '娜': 0, '杰': 0, '昂': 0,
        '扬': 0, '尚': 0, '俊': 0, '巨': 0, '亮': 0
      }
    };
  },
  mounted() {
    const today = new Date();
    const month = today.getMonth() + 1; // 月份从0开始
    const day = today.getDate();
    this.currentDate = `${month}月${day}日`;
  },
  methods: {
    setCurrentIndex(index,column) {
      this.currentIndex = index;
      this.currentColumn = column;
    },
    fillColumn(person) {
      console.log('Fill Column:', person);
      if (this.currentIndex !== null && this.personUsage[person] < 6) {
        console.log('Current Index:', this.currentIndex);
        if (this.currentColumn === 'B') {
          this.schedules[this.currentIndex].b = person;
        } else if (this.currentColumn === 'C') {
          this.schedules[this.currentIndex].c = person;
        }
        this.personUsage[person]++;
        console.log('Person Usage:', this.personUsage[person]);
        this.moveToNextCell();
      }
    },
    incrementUsage(person) {
      if (person && this.personUsage[person] !== undefined) {
        this.personUsage[person]++;
      }
    },
    moveToNextCell() {
      this.currentIndex++;
      console.log('Move to Next Cell, Current Index:', this.currentIndex);
      if (this.currentColumn === 'B' && this.currentIndex >= this.schedules.length) {
        this.currentIndex = 0;
        this.currentColumn = 'C';
      } else if (this.currentColumn === 'C' && this.currentIndex >= this.schedules.length) {
        this.currentIndex = null; // 填充完毕，重置索引
      }
      console.log('Current Column:', this.currentColumn);
    },
    clearTable() {
      // 清空表格中的数据
      this.schedules = this.schedules.map(schedule => ({ ...schedule, b: '', c: '' }));
      this.currentIndex = null;
      this.currentColumn = 'B';
      // 不重置标签使用次数
    }
  },
}
</script>

<style>
/* 适应手机屏幕 */
#app {
  font-family: 'Helvetica Neue', Arial, sans-serif;
  background-color: #f9f9f9;
  padding: 2px;
  max-width: 100%;
  box-sizing: border-box;
}

/* 头部样式 */
.header {
  display: flex;
  align-items: center;
  margin-bottom: 2px;
}

h1 {
  flex:2;
  font-size: 24px;
  color: #333;
  margin: 0;
  text-align:center;
  white-space: nowrap; /* 防止文本换行 */
}

.location-select {
  font-size: 16px;
  padding: 5px;
  border-radius: 4px;
  border: 1px solid #ddd;
  background-color: #fff;
  color: #333;
  margin-left:auto;/* 将select靠右对齐 */
}

/* 人员标签样式 */
.tags {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  margin-bottom: 5px;
}

.tag {
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: #007BFF;
  color: white;
  padding: 4px 6px;
  margin: 2px;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s ease;
  text-align: center;
}

.tag:hover {
  background-color: #0056b3;
}

.tag.disabled {
  background-color: #ccc;
  cursor: not-allowed;
}

/* 列表样式 */
.schedule-list {
  background-color: #fff;
  border: 1px solid #ddd;
  border-radius: 4px;
  overflow: hidden;
}

/* 列表行样式 */
.time-row {
  display: flex;
  justify-content: space-between;
  align-items: center;
  border-bottom: 1px solid #eee;
  padding: 4px 8px;
  transition: background-color 0.3s ease;
  text-align: center;
}

.time-row:hover {
  background-color: #f1f1f1;
}

.header-row {
  background-color: #007BFF;
  color: white;
  font-weight: bold;
}

.time-slot,
.column-header {
  font-size: 16px;
  color: #333;
  width: 30%;
  text-align: center;
}

  /* 列输入框样式 */
.column-input {
  font-size: 16px;/* 调整字体大小 */
  color: #333;
  width: 30%;
  border: 1px solid #ddd;
  padding: 4px;/* 减少内边距，减小高度 */
  border-radius: 4px;
  text-align: center;
  box-sizing: border-box;
  transition: border-color 0.3s ease;
}

.column-input:focus {
  border-color: #007BFF;
  outline: none;
}

.special-times {
  background-color: #fafafa;
}
</style>
