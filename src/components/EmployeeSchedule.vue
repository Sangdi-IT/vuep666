<!-- src/components/EmployeeSchedule.vue -->
<template>
  <div class="schedule-item">
    <span class="time-slot">{{ schedule.timeSlot }}</span>
    <span class="employee">{{ schedule.employee }}</span>
    <button @click="$emit('edit', schedule)">编辑</button>
    <button @click="$emit('delete', schedule)">删除</button>
  </div>
</template>

<script>
export default {
  name: 'EmployeeSchedule',
  props: {
    schedule: Object,
  },
};
</script>

<style scoped>
.schedule-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  border-bottom: 1px solid #DDDDDD;
  padding: 10px;
  background-color: #FFFFFF;
  margin-bottom: 10px;
}

.schedule-item .time-slot,
.schedule-item .employee {
  font-size: 16px;
  color: #333333;
}

.schedule-item button {
  margin-left: 10px;
  padding: 5px 10px;
  background-color: #007BFF;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}
</style>
